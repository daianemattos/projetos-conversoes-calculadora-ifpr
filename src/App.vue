<template>
  <v-app>
    <v-main>
      <v-container fill-height fluid>
        <v-row align="center" justify="center">
          <v-card
            width="900"
          >
            <v-card-title
              class="primary white--text titulo"
            >
              Calculadora Binária
            </v-card-title>
            <v-card-text class="pa-6">
              <v-row align="center" justify="center" class="pa-5">
                <v-col cols="6">
                  <v-select
                    v-model="itemSelect"
                    :items="itemsSelect"
                    label="Opções"
                    outlined
                    @change="limpar"
                  ></v-select>
                </v-col>
              </v-row>

              <v-row v-if="itemSelect === 'conversoes'" align="center" justify="center" class="pa-2">
                <v-col cols="12" md="6">
                  <v-select
                    v-model="itemConversoes"
                    :items="itemsConversoes"
                    label="Converter"
                    outlined
                    @change="limpar"
                  ></v-select>
                </v-col>
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="valor"
                    label="Valor"
                    outlined
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row  v-if="itemSelect === 'conversoes'" align="center" justify="center" class="pa-2">
                <v-btn
                  color="success"
                  align="center"
                  large
                  :disabled="!itemConversoes || !valor"
                  @click="converter"
                >
                  Converter
                </v-btn>
              </v-row>

              <v-row v-if="itemSelect === 'calculos'" align="center" justify="center" class="pa-2">
                <v-col cols="12" md="5">
                  <v-text-field
                    v-model="valor1"
                    label="Valor 1"
                    outlined
                  ></v-text-field>
                </v-col>
                <v-col cols="12" md="2">
                  <v-select
                    v-model="itemCalculadora"
                    :items="itemsCalculadora"
                    label="Operação"
                    outlined
                  ></v-select>
                </v-col>
                <v-col cols="12" md="5">
                  <v-text-field
                    v-model="valor2"
                    label="Valor 2"
                    outlined
                  ></v-text-field>
                </v-col>
              </v-row>
              <v-row  v-if="itemSelect === 'calculos'" align="center" justify="center" class="pa-2">
                <v-btn
                  color="success"
                  align="center"
                  large
                  :disabled="!valor1 || !valor2"
                  @click="calcular"
                >
                  Calcular
                </v-btn>
              </v-row>
              <v-row align="center" justify="center" class="pa-2">
                <v-textarea
                  v-model="resultado"
                  label="Resultado"
                  class="mt-5 pa-3"
                  outlined
                  rows="2"
                  readonly
                >
                </v-textarea>
              </v-row>
              <v-row align="center" justify="center" class="pa-2 text-center footer">
                Trabalho desenvolvido na disciplina de Organização e Arquitetura de Computadores
              </v-row>
            </v-card-text>
          </v-card>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'Conversoes',

  data: () => ({
    itemSelect: 'conversoes',
    itemConversoes: null,
    itemCalculadora: null,
    valor: null,
    valor1: null,
    valor2: null,
    resultado: null,
    
    itemsSelect: [{
      text: 'Conversões', value: 'conversoes'
    }, {
      text: 'Cálculos', value: 'calculos' 
    }],

    itemsConversoes: [{
      text: 'Decimal para Binário', value: 'decimal_binario'
    }, {
      text: 'Decimal para Octal', value: 'decimal_octal'
    }, {
      text: 'Decimal para Hexadecimal', value: 'decimal_hexadecimal'
    }, {
      text: 'Binário para Decimal', value: 'binario_decimal'
    }, {
      text: 'Octal para Decimal', value: 'octal_decimal'
    }, {
      text: 'Hexadecimal para Decimal', value: 'hexadecimal_decimal'
    }],

    itemsCalculadora: [{
      text: '+', value: 'adicao'
    }, {
      text: '-', value: 'subracao'
    }, {
      text: 'x', value: 'multiplicacao'
    }, {
      text: '/', value: 'divisao'
    }]
  }),

  methods: {
    limpar() {
      this.resultado = null
    },

    converter() {
      switch (this.itemConversoes) {
        case 'decimal_binario':
          this.decimalPara(this.valor, 2)
          break 
        case 'decimal_octal':
          this.decimalPara(this.valor, 8)
          break
        case 'decimal_hexadecimal':
          this.decimalPara(this.valor, 16)
          break
        case 'binario_decimal':
          this.binarioParaDecimal(this.valor)
          break
        case 'octal_decimal':
          this.octalParaDecimal(this.valor)
          break
        case 'hexadecimal_decimal':
          this.hexadecimalParaDecimal(this.valor)
          break
      }
    },

    calcular() {
      switch (this.itemCalculadora) {
        case 'adicao':
          this.somar(this.valor1, this.valor2)
          break
        case 'subracao':
          this.subtrair(this.valor1, this.valor2)
          break
        case 'multiplicacao':
          this.multiplicar(this.valor1, this.valor2)
      }
    },

    decimalPara(num, base) {
      if (num > 0) {
        let resultado = ''
        while (num > 0) {
          let resto = num % base
          if (base === 16) {
            switch(resto) {
              case 10: 
                resto = 'A'
                break
              case 11:
                resto = 'B'
                break
              case 12:
                resto = 'C'
                break
              case 13:
                resto = 'D'
                break
              case 14: 
                resto = 'E'
                break
              case 15: 
                resto = 'F'
                break
            }
          }
          num = parseInt(num / base)
          resultado = resto + resultado
        }
        this.resultado = resultado
      } else {
        this.resultado = 'Valor inválido!'
      }
    },

    binarioParaDecimal(num) {
      if (num >= 0) {
        let numero = num.split('')
        try{
          numero.forEach(n => {
          if (n != 0 && n != 1) {
            this.resultado = 'Valor inválido!'
            throw 'Valor inválido!'
          } else {
            let decimal = 0
            for (let i = 0; i < num.length; i++) {
              if (num[num.length - (i + 1)] === '1') {
              decimal += 2 ** i
              }
            }
            this.resultado = decimal
          }
        }) 
        } catch(e) {
          this.resultado = 'Valor inválido!'
        }
      } else {
        this.resultado = 'Valor inválido!'
      }
    },

    octalParaDecimal(num) {
      if (num > 0) {
        if(!num.includes(8) && !num.includes(9)) {
          let decimal = 0
          for (let i = 0; i < num.length; i++) {
            if (num[num.length - (i + 1)] !== '0') {
              decimal += (8 ** i) * num[num.length - (i + 1)]
            }
          }
          this.resultado = decimal
        } else {
        this.resultado = 'O valor não pode conter 8 ou 9.'
        }
      } else {
        this.resultado = 'Valor inválido!'
      }

    },

    hexadecimalParaDecimal(num) {
      let valor = num.split('')
      let v = ''

      try {
        valor.forEach(n => {
          if (!['A', 'B', 'C', 'D', 'E', 'F', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'].includes(n)) {
            this.resultado = 'Valor inválido!'
            throw 'Valor inválido!'
          } else {
            v += n
          }
        })

        let numero = v

        numero = numero.replaceAll('A', '10')
                      .replaceAll('B', '11')
                      .replaceAll('C', '12')
                      .replaceAll('D', '13')
                      .replaceAll('E', '14')
                      .replaceAll('F', '15')

        let decimal = 0

        for (let i = 0; i < numero.length; i++) {
          if (numero[numero.length - (i + 1)] !== '0') {
            decimal += (16 ** i) * numero[numero.length - (i + 1)]
          }
        }
        this.resultado = decimal
      } catch (e) {
        this.resultado = 'Valor inválido!'
      }
    },

    verificarBinario (num) {
      num = num + ''
      let result = true
      num.split('').forEach(n => {
        if (n != '1' && n != '0') {
          result = false
        }
      })
      return result     
    },

    somar(valor1, valor2) {
      if (!this.verificarBinario(valor1) || !this.verificarBinario(valor2)) {
        this.resultado = 'Valor inválido'
        return false
      }

      if (valor1?.length > valor2?.length) {
        valor2 = valor2.padStart(valor1.length, '0')
      } else if (valor2?.length > valor1?.length) {
        valor1 = valor1.padStart(valor2.length, '0')
      }

      let array1 = valor1.split('').reverse()
      let array2 = valor2.split('').reverse()
      let tamanho = array1?.length
  
      let resto = 0
      let resultado = ''
      for (let i = 0; i < tamanho; i++) {
        let n1 = array1[i]
        let n2 = array2[i]

        if (n1 == 0 && n2 == 0) {
          if (resto == 1) {
            resultado = '1' + resultado
            resto = 0
          } else {
            resultado = '0' + resultado
          }
        } 
        else if ((n1 == 0 && n2 == 1) || (n1 == 1 && n2 == 0)) {
          if (resto == 1) {
            resultado = '0' + resultado
            resto = 1
          } else {
            resultado = '1' + resultado
          }
        } 
        else if (n1 == 1 && n2 == 1) {
          if (resto == 1) {
            resultado = '1' + resultado
            resto = 1
          } else {
            resultado = '0' + resultado
            resto = 1
          }
        }
      }
      if (resto === 1) resultado = '1' + resultado

      this.resultado = this.itemCalculadora === 'adicao' ? resultado : ''
      return resultado
    },

    subtrair(valor1, valor2) {
      if (!this.verificarBinario(valor1) || !this.verificarBinario(valor2)) {
        this.resultado = 'Valor inválido'
        return false
      }

      let maior = ''
      let negativo = false

      if (valor1.length > valor2.length) {
        maior = valor1.length
        valor2 = valor2.padStart(valor1.length, '0')
      } else {
        maior = valor2.length
        valor1 = valor1.padStart(valor2.length, '0')
      }

      let complemento = this.complementar(valor2)
      let resultado = this.somar(complemento, valor1)
      
      for (let i = 0; i < valor1.length; i++) {
        if (valor1[i] !== valor2[i]) {
          if (valor1[i] == 0) {
            negativo = true;
          }
          break;
        }
      }

      if (negativo) {
        resultado = this.complementar(resultado)
        if (resultado.length > maior) {
          let remove = resultado.length - (maior)
        }
      } else {
        if (resultado.length > maior) {
          let remove = resultado.length - (maior)
          resultado = resultado.substring(remove, resultado.length);
        }
      }
      this.resultado = resultado
    },

    multiplicar(valor1, valor2) {
      if (!this.verificarBinario(valor1) || !this.verificarBinario(valor2)) {
        this.resultado = 'Valor inválido'
        return false
      }

      if (valor1.length > valor2.length) {
        valor2 = valor2.padStart(valor1.length, '0')
      } else {
        valor1 = valor1.padStart(valor2.length, '0')
      }
      
      let resultado = '0';

      for(let i = valor2.length - 1, j = 0; i >= 0; i--, j++) {

          if(valor2[i] === '1') {
            resultado = this.somar(resultado, valor1 + "0".repeat(j));
            console.log(resultado)
          }
      }

      this.resultado = resultado;
    },

    complementar(valor) {
      let result = ''
      for (let i = 0; i < valor.length; i++) {
        let aux = valor[i]
        if (aux == 0) aux = aux.replace('0', '1')
        else aux = aux.replace('1', '0')
        result += aux
      }

      result = this.somar(result, '1')
      return result
    }
  }
}
</script>

<style>
  @media(max-width:500px) {
    .titulo {
      font-size: 1rem !important;
    }

    .footer {
      font-size: 0.85rem !important;
    }
  }
</style>