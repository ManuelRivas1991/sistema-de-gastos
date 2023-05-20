<script>

export default {
    data(){
        return{
            record: false,
            nuevacategoria:'',
            nuevoArticulo:'',
            nuevoPrecio:0,
            today: '',
            pending:[
                {
                    category: 'Supermercado', 
                    item: 'Arroz', 
                    price: 50, 
                    date: 'Sat May 20 2023 08:28:55 GMT-0300 (hora estándar de Uruguay)',
                    pay: false,
                    datePay:'' 
                },
                {
                    category: 'Supermercado', 
                    item: 'Carne', 
                    price: 100, 
                    date: 'Sat May 20 2023 08:33:45 GMT-0300 (hora estándar de Uruguay)',
                    pay: false,
                    datePay:'' 
                },
                {
                    category: 'UTE', 
                    item: 'Factura',
                    price: 3000, 
                    date: 'Sat May 20 2023 08:44:54 GMT-0300 (hora estándar de Uruguay)',
                    pay: false,
                    datePay:'' 
                }

            ]
            
        }
    },
  computed: {
   
    printTitle() {
    
      return this.record ? 'Historial de Pagos' : 'Sistema de Gastos'
    },
    obj(){
        
        return {
                    category: this.nuevacategoria,
                    item: this.nuevacategoria!== 'Supermercado' ? 'Factura' : this.nuevoArticulo,
                    price: this.nuevoPrecio,
                    date: this.today,
                    pay: false,
                    datePay:''   
                }
    },
    printClick(){
        return this.record ? 'Click para deshacer pago' : 'Click para pagar'
    },
    Total(){
        let total =0;
        this.pending.forEach(element => {
            if(element.pay === this.record){
                total +=  element.price
            }
        });
        return total
    },
    printTotal(){  
        return this.record ? `Total pagado: ${this.Total} pesos` : `Total a pagar: ${this.Total} pesos`
    }

  },
    methods: {
        nowClick : function () {
            const timeElapsed = Date.now();
            const now = new Date(timeElapsed);
            return now.toString()
        },
        cargar : function () {
            if(this.nuevacategoria&&this.nuevoPrecio){ 
               switch (this.nuevacategoria) {
                case 'Supermercado':
                    if(this.nuevoArticulo){
                        this.today =this.nowClick()
                        this.pending.push(this.obj)
                    }
                    break;
               
                default:
                    this.today =this.nowClick()
                    this.pending.push(this.obj)
                    break;
               }  
                
            }
        },
        payClick : function (purchase) {
            purchase.pay = !purchase.pay; 
            purchase.datePay = this.nowClick()
        }
    }

}


</script>

<template>
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container-fluid">
    <div class="row col ">
        <div class="col ">
            <button class="btn btn-outline-success me-5" v-on:click="record = false">Sistema de Gastos</button>
            <button class="btn btn-outline-success" v-on:click="record = true">Historial de Pagos</button>
        </div>
    </div> 
  </div>
</nav>
  <div class="bg-light p-5 rounded">
    <h1 class="no-seleccionable">{{printTitle}}</h1>

    <div class="row my-3">
        <div class="col">
            <div class="input-group">
                <select class="form-select" v-model="nuevacategoria" >
                    <option value="" selected disabled>Seleccione categoría  ...</option>
                    <option value="Supermercado">Supermercado</option>
                    <option value="UTE">UTE</option>
                    <option value="OSE">OSE</option>
                    <option value="ANTEL">ANTEL</option>
                </select>
                <input type="text" class="form-control" placeholder="Articulo" v-model="nuevoArticulo">
                <input type="number" class="form-control form-icon-trailing" placeholder="Precio" v-model="nuevoPrecio"/>
                <button class="btn btn-outline-secondary" type="button" v-on:click=" cargar">Agregar Compra</button>
            </div>
        </div>
    </div>

    <template v-for="(purchase,index) in pending" :key="index">
        <div class="list-group" v-if="purchase.pay === record">
        <a class="list-group-item list-group-item-action flex-column align-items-start" v-on:click="payClick(purchase)">
            <div class="d-flex w-100 justify-content-between">
                <h4 class="mb-1 no-seleccionable">{{purchase.category}}</h4>
                <small class="text-muted no-seleccionable" v-if="!record">{{  purchase.date }}</small>
                <small class="text-muted no-seleccionable" v-if="record">{{  purchase.datePay }}</small>
            </div>
            <small class="text-muted no-seleccionable">{{ printClick }}</small>
            <h5 class="mb-1 no-seleccionable ">Articulo: {{purchase.item}} - Precio: {{purchase.price}}</h5>
        </a>
        </div>
    </template>
    <h1 class="no-seleccionable mt-5">{{printTotal}}</h1>
  </div>
</template>
<style>
.no-seleccionable {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none; 
}

</style>