<template>
  <div id="app">
    <div class="container__spinner" >
      <div class="spinner">
      </div>
      <div class="message__spinner">
        <span>Cargando editor</span> <img src="@/assets/markdown.png" alt="">...
      </div>
    </div>

    <codemirror ref="myCm"
      :value="code" 
      :options="cmOptions"
      @input="onCmCodeChange">
    </codemirror>

    <markdown-it-vue class="md-body" :options="options" :content="code" />
  </div>
</template>

<script>
import 'codemirror/mode/markdown/markdown.js'
import 'codemirror/theme/ayu-dark.css'


import MarkdownItVue from 'markdown-it-vue'
import 'markdown-it-vue/dist/markdown-it-vue.css'

export default {
  data () {
    return {
      code: `# your markdown content`,
      options: {
        markdownIt: {
          linkify: true
        },
        linkAttributes: {
          attrs: {
            target: '_blank',
            rel: 'noopener'
          }
        }
      },
      cmOptions: {
        // codemirror options
        tabSize: 5,
        mode: 'text/x-markdown',
        theme: 'ayu-dark',
        lineNumbers: true,
        line: true,
      }
    }
  },
  components: {
    MarkdownItVue
  },
  methods: {
    onCmCodeChange(newCode) {
      this.code = newCode
    }
  },
  computed: {
    codemirror() {
      return this.$refs.myCm.codemirror
    }
  },
  mounted() {
    const container_spinner = document.querySelector('.container__spinner')
    setTimeout(() => {
        container_spinner.classList.add('disabled')
    }, 2500);


    window.onresize = () => {
        let width = window.innerWidth
        document.querySelector('.vue-codemirror').style.width = `${width/2}px`
        document.querySelector('.markdown-body').style.width = `${width/2}px`
    }
  },
}
</script>

<style>
html,body{
  height: 100%;
}

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app{
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  height: 100%;
}

.CodeMirror{
  height: 100% !important;
}

.vue-codemirror{
  overflow: hidden;
}

.CodeMirror-scroll,
.markdown-body{
  padding: 10px 24px;
  height: 100%;
  overflow: hidden;
}

.markdown-body{
  max-height: 100%;
  overflow-y: auto;
}

.container__spinner{
  position: absolute;
  background-color: white;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.message__spinner{
  margin-top: 10px;
  font-size: 20px;
  color: #6f6f6f;
  display: flex;
  align-items: center;
}

.message__spinner>img{
  margin-left: 5px;
}

.spinner{
  border: 7px solid rgba(0, 0, 0, .1);
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border-left-color: rgb(85, 216, 135);
  transition: 1s;
  animation: spin 1s ease infinite;
}

.disabled{
  transition: .5s;
  animation: degrade .5s ease both;
  z-index: -10;
}

.monaco-editor{
  width: 100%;
}

@keyframes degrade {
  0%{
    opacity: 1;
  }

  100%{
    opacity: 0;
  }
}


@keyframes spin {
  0%{
    transform: rotate(0deg);
  }

  100%{
    transform: rotate(360deg);
  }
}
</style>
