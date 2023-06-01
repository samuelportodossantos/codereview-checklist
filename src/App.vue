<template>
  <div id="app">

    <input type="text" v-model="assign" placeholder="assinatura">
    <br/>

    <a target="_blank" href="https://github.com/samuelportodossantos/codereview-checklist">Github</a>

    <div class="alert-message">⚠️ Lembrar de aprovar o merge request</div>

    <table>
      <caption>Review checklist</caption>
      <thead>
        <tr>
          <th>Answer</th>
          <th>Question</th>
        </tr>
      </thead>
      <tbody>
        <QuestionRow v-for="item in checklist" :uid="item.id" @handlestatus="setResponse" :status="item.defaultStatus" :question="item.question" :key="item.id" @response="setResponse"/>
      </tbody>
    </table>


    <div class="generate-btn" @click="generateMarkdown()">Copy to clipboard</div>
  </div>
</template>

<script>
import QuestionRow from './components/QuestionRow.vue';


export default {
    name: "App",
    components: { QuestionRow },
    data() {
      return {
        assign: '',
        checklist: [
          {
            id: 1,
            defaultStatus: 2,
            question: "Checou com o DEV se o problema foi REALMENTE reproduzido?"
          },
          {
            id: 2,
            defaultStatus: 2,
            question: "O código está legível e não precisa ser melhorado?"
          },
          {
            id: 3,
            defaultStatus: 2,
            question: "O SonarQube está com algum erro?"
          },
          {
            id: 4,
            defaultStatus: 2,
            question: "Os testes unitários estão funcionando? (caso existam)"
          },
          {
            id: 5,
            defaultStatus: 2,
            question: "As consultas de bancos de dados estão otimizadas? (caso necessário)"
          },
          {
            id: 6,
            defaultStatus: 2,
            question: "O código utiliza as melhores práticas de segurança?"
          },
          {
            id: 7,
            defaultStatus: 2,
            question: "O código está resiliente em caso de erros as informações essenciais não serão perdidas?"
          },
          {
            id: 8,
            defaultStatus: 2,
            question: "Foi adicionado logs essenciais e/ou logs de debug?"
          },
          {
            id: 9,
            defaultStatus: 2,
            question: "Foi verificado que a implementação não vai gerar Loops infinitos?"
          },
          {
            id: 10,
            defaultStatus: 2,
            question: "Foi adicionado tratamento de exceções? (caso necessário)"
          },
          {
            id: 11,
            defaultStatus: 2,
            question: "Foi adicionado verificação da existência keys de arrays/objetos antes de acessá-las?  (caso necessário)"
          },
          {
            id: 12,
            defaultStatus: 2,
            question: "Foi utilizado técnicas de cacheamento? (caso necessário)"
          },
          {
            id: 13,
            defaultStatus: 2,
            question: "Em caso de milhares de dados consultados o código se comportará da maneira adequada?"
          },
          {
            id: 14,
            defaultStatus: 2,
            question: "Foi utilizado paginação? (caso necessário)"
          },
          {
            id: 15,
            defaultStatus: 2,
            question: "Foi validado o impacto em Whitelabel?"
          },
          {
            id: 16,
            defaultStatus: 2,
            question: "Foi analisado contexto do uso do banco de dados?"
          },
          {
            id: 17,
            defaultStatus: 2,
            question: "Foi implementado filas/schedulers/jobs da maneira correta e otimizada, como por exemplo utilizando o Temporal com as melhores práticas?"
          },
          {
            id: 18,
            defaultStatus: 2,
            question: "Em caso de milhares de requisições o código se comportará da maneira adequada (Teste de Carga)?"
          }
        ]
      }
    },
    mounted() {
      this.assign = localStorage.getItem('assign')
    },
    methods: {
      setResponse(event) {
        const index = this.checklist.findIndex(element => element.id == event.uid)
        if (index !== -1) {
          this.checklist[index].defaultStatus = event.status
        }

      },
      getIconByStatus(status){
        const statusList = {
          0: ':white_check_mark:',
          1: ':x:',
          2: ':large_blue_circle:'
        }
        return statusList[status] || statusList[2]
      },
      generateMarkdown(){
        let text = `<h2>Code Review Sz.Chat</h2>
        <ul>`
        
        this.checklist.map(element => {
          let icon = this.getIconByStatus(element.defaultStatus)
          text += `<li>${icon} ${element.question}</li>`
        })

        text += `</ul><hr>
        <strong>Legenda</strong> <br/><br/>
          <ul>
            <li>:white_check_mark: Ok</li>
            <li>:x: Precisa ajustar</li>
            <li>:large_blue_circle: Não se aplica a atividade</li>
          </ul> <br/>`

        text += `
          Assign: ${this.assign}
        `
        navigator.clipboard.writeText(text)
        console.log(this.assign);
        localStorage.setItem('assign', this.assign)
      }
    }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.d-flex {
  display: flex;
}

.d-direction-column {
  flex-direction: column;
}

.al-left {
  align-items: flex-start;
}

table {
  border: 1px solid black;
  width: 100%;
}

.generate-btn {
  position: fixed;
  right: 30px;
  bottom: 30px;
  padding: 20px;
  background-color: tomato;
  color: white;
  text-transform: uppercase;
  border-radius: 5px;
}

table td, table th {
  border: 1px solid black;
  font-size: 20px;
  width: max-content;
  padding: 5px 20px;
  text-align: left;
}

input, label {
  cursor: pointer;
}

label {
  width: 100%;
  text-align: left;
  padding: 5px 0;
  font-size: 20px;
}

label:hover{
  background-color: #000;
  color: white;
}

td div {
  width: 100%;
  display: flex;
  justify-content: flex-start;
}

.alert-message{
  padding: 20px;
  background-color: #ff9431;
  border: 2px solid #7e4237;
  border-radius: 5px;
  margin-bottom: 10px;
  margin-top: 10px;
  font-size: 20px;
  color: white;
}
</style>
