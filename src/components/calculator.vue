<template>
  <div>
      <div><input type='hidden' :value="previous">
            <input type='hidden' :value="operator">
            <input type='hidden' :value="current">
            <br/>
      </div>
        
      <div class="container">           
          <div class='row resultdisplay'>
              <div class="col-md-12">{{display || '0'}}</div>
          </div>
          <div class='row'>
              <div class="col" @click="resetVal">C</div>
              <div class="col" @click="sign">+/-</div>
              <div class="col" @click="setOperator('%')">%</div>
              <div class="col orgbgk" @click="setOperator('/')">/</div>
          </div>
          <div class='row'>
              <div class="col" @click="setNumber(7)">7</div>
              <div class="col" @click="setNumber(8)">8</div>
              <div class="col" @click="setNumber(9)">9</div>
              <div class="col orgbgk" @click="setOperator('*')">*</div>
          </div>
          <div class='row'>
              <div class="col" @click="setNumber(4)">4</div>
              <div class="col" @click="setNumber(5)">5</div>
              <div class="col" @click="setNumber(6)">6</div>
              <div class="col orgbgk" @click="setOperator('-')">-</div>
          </div>  
          <div class='row'>
              <div class="col" @click="setNumber(1)">1</div>
              <div class="col" @click="setNumber(2)">2</div>
              <div class="col" @click="setNumber(3)">3</div>
              <div class="col orgbgk" @click="setOperator('+')">+</div>
          </div>
          <div class='row'>
              <div class="col"></div>
              <div class="col" @click="setNumber(0)">0</div>
              <div class="col" @click="setNumber('.')">.</div>
              <div class="col orgbgk" @click="equalTo">=</div>
          </div> 
      </div>
  </div>
</template>

<script>
export default {
  data() {
      return{
        previous: '',
        current: '',
        display: '',
        operator: '',
        operatorSelected: false,
      }            
  },
  methods: {
      resetVal(){
          this.current    = '';
          this.display    = '';
          this.previous   = '';
          this.operator   = '';
          this.operatorSelected = false;
      },
      setNumber(num){
          if(num == '.' && this.current.indexOf('.') !== -1) {
              return false;
          }
          if(this.operatorSelected){
              this.current            = '';
              this.operatorSelected   = false;
          }            
          this.current += ''+num;        
          this.display = this.current;
      },
      setOperator(op){      
          if(this.current){
              //1st time pressed :    set current to previous and make current blank
              if(!this.previous){
                  this.previous   = this.current;
                  this.current    = '';                    
              }else{
                  //2nd time pressed :    perform op
                                          //perfor old op, say previous = previous + current
                                          //make current blank
                                          //display = previous
                                          //update operator this.operator = op
                                          //update operatorSelected = true
                  this.previous   = this.performOp();
                  this.current    = '';
                  this.display    = this.previous;
              }
          }                
          this.operator   = op;
          this.operatorSelected   = true;
      },
      equalTo(){                  
          this.previous   = this.performOp();
          this.current    = '';
          this.display    = this.previous;
      },
      performOp(){ 
          if(this.operator == '+'){                    
              return parseFloat(this.previous) + parseFloat(this.current);
          }else if(this.operator == '-'){
              return parseFloat(this.previous) - parseFloat(this.current);
          }else if(this.operator == '/'){
              return parseFloat(this.previous) / parseFloat(this.current);
          }else if(this.operator == '*'){
              //this.previous = parseFloat(this.previous) + parseFloat(this.current);
              return parseFloat(this.previous) * parseFloat(this.current);
          }else if(this.operator == '%'){
              return parseFloat(this.previous) % parseFloat(this.current);
          }
      },
      sign(){
          this.display = this.display.charAt(0) === '-' ? 
          this.display.slice(1) : `-${this.display}`;
          if(this.current){
            this.current = this.display;
          }else{
            this.previous = this.display;
          }
      },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .row{
        outline: 1px solid grey;
    }
    .col{
        outline: 1px solid grey;
    }
    .orgbgk{
        background-color: orange;
    }
    .resultdisplay{
      background-color:midnightblue;
      color: white;
    }
</style>
