<template>
  <div>
    <p>
      Data wystawienia: <span contenteditable>01.01.2018, Miejscowość</span>
    </p>
    <h1 contenteditable>Rachunek Nr 1</h1>
    
    
    <div class="zleceniobiorca">
      <h2>Sprzedawca:</h2>
      <p contenteditable>
        Firma <br>
        Ulica 1 <br>
        00-000 Miasto<br>
        NIP 000-000-00-00
      </p>
    </div>
    
    <div class="zleceniodawca">
      
      <p>
          <select class="opt">
            <option value="zd1">kontrahent 1</option>
            <option value="zd2">kontrahent 2</option>
            <option value="zd2">kontrahent 3</option>
          </select>
      </p>
      
      <h2>Nabywca:</h2>
      <p contenteditable>
        Firma <br>
        Ulica 1 <br>
        00-000 Miasto<br>
        NIP 000-000-00-00
      </p>
    </div>
    
    <table class="table">
      <thead>
        <th>Lp.</th>
        <th>Nazwa towaru lub usługi</th>
        <th>Cena</th>
        <th>Ilość</th>
        <th>Cena razem</th>
      </thead>
       <tbody>
          <tr v-for="(item, index) in items" v-bind:key="item.id">
            <td class="table__no">{{index+1}}.</td>
            <td class="table__title">
              <div contenteditable>{{item.title}}</div>
              <div class="g--screen">
                <button @click="addRow(index)">+</button>
                <button @click="removeRow(index)">-</button>
              </div>
            </td>
            <td class="table__price">
              <input class="g--screen" v-model.number="item.price"> 
              {{item.price | price}} zł
            </td>
            <td class="table__qty">
              <input type="number" v-model.number="item.qty">
            </td>
            <td class="table__price">
              <!-- <input class="g--screen" v-model.number="subtotalRow[index]" readonly>  -->
              {{ subtotalRow[index] | price }} zł
            </td>
         </tr>
         <tr>
           <td class="t--right" colspan="4"><strong>Razem</strong></td>
           <td class="table__price">
             <strong>{{ total | price }} zł</strong>
           </td>
         </tr>
       </tbody>
     </table>
   </div>
</template>

<script>
export default {
  name: 'Invoice',
  data() {
    const data = {
      items: [
        {title: 'Nazwa towaru lub usługi', qty: 1, price: 10},
        {title: 'Nazwa towaru lub usługi', qty: 1, price: 20}
      ]
    };
    return data;
  },
  computed: {
    subtotalRow() {
      return this.items.map((item) => {
        return Number(item.qty * item.price)
      });
    },
    total() {
      return this.items.reduce((total, item) => {
        return total + item.qty * item.price;
      }, 0);
    }
  },
  methods: {
    addRow(index) {
      this.items.splice(index + 1, 0, {
        title: 'Nazwa towaru lub usługi', qty: 1, price: 0
      });
    },
    removeRow(index) {
      this.items.splice(index, 1);
    }
  },
  filters: {
    price(value) {
      return value.toFixed(2).replace('.', ',');
    }
  }
}
</script>

<style scoped>
  h1  { text-align: center; font-size: 1.25em; margin: 1em 0; }
  h2 { font-size: 1em; margin: 0 0 -1em; }
  .zleceniobiorca {}
  .zleceniodawca { 
    /* text-align: right;  */
  }

  table { border-collapse: collapse; width: 100%; margin-top: 32px; }
  th, td { border: 1px solid #ccc; padding: 0.5em 1em; text-align: center; }
  .table__no { width: 1%; }
  .table__title { width: 300px; text-align: left; }
  .table__price { text-align: right; white-space: nowrap; }
  
  input { font-size: inherit; font-family: inherit; width: 100px; border: 0; background: #fdfddf; }
  
  .table__qty { width: 20px; }
  .table__qty input { text-align: center; }
  
  .zd-hidden { display: none; }
  
  .t--right { text-align: right; }
  
  [contenteditable="true"] { background: #fdfddf; }

  @media print {
    .opt { display: none; }
    .controls { display: none; }
    .g--screen { display: none; }
    .table__qty input { border: 0; padding: 0; width: 20px; }
    
    [contenteditable="true"] { background: transparent; }
  }
  
  
  
  
</style>
