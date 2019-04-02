<template>
  <div>
    <select v-model="selected_pdf">
      <option v-for="file in pdfs" :value=file.file>{{file.name}}</option>
    </select>

    <div id="pdfvuer">
      <!--Father forgive me for I have sinned. The :key is horrible here. Figure out better handling.-->
      <pdf :src="pdfdata" v-for="i in numPages" :key=getRandomInt() :id="i" :page="i"
        :scale.sync="scale" style="width:100%;margin:20px auto;">

        <template slot="loading">
          loading content here...
        </template>

      </pdf>
    </div>
  </div>
</template>

<script>
import pdfvuer from 'pdfvuer'

//bs handling required by webpack
import monkey from '../assets/tracemonkey.pdf'
import lorem from '../assets/lorem-ipsum.pdf'

export default {
    name:'Demo',
  components: {
    pdf: pdfvuer
  },
  data () {
    return {
      page: 1,
      numPages: 0,
      pdfdata: undefined,
      errors: [],
      scale: 'page-width',
        //Add new Pdfs here.
        pdfs:[{'name':'tracemonkey', 'file':monkey},{'name':'lorem', 'file':lorem}],
        selected_pdf:lorem
    }
  },
  computed: {
    formattedZoom () {
        return Number.parseInt(this.scale * 100);
    },
  },
  mounted () {
    this.getPdf()
  },
  watch: {
    show: function (s) {
      if(s) {
        this.getPdf();
      }
    },
      selected_pdf: function(s){
        if(s) {
        this.getPdf();
      }
      }
    ,
    //  Add some scrolling functionality
    page: function (p) {
      if( window.pageYOffset <= this.findPos(document.getElementById(p)) || ( document.getElementById(p+1) && window.pageYOffset >= this.findPos(document.getElementById(p+1)) )) {
        document.getElementById(p).scrollIntoView();
      }
    }
  },
  methods: {
    getPdf () {
      var self = this;
      self.pdfdata = pdfvuer.createLoadingTask(this.selected_pdf);
      self.pdfdata.then(pdf => {
        self.numPages = pdf.numPages;
      });
    },
    findPos(obj) {
      return obj.offsetTop;
    },
      getRandomInt() {
        return Math.floor(Math.random() * Math.floor(9999));
}

  }
}
</script>
