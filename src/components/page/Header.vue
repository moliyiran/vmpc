<template>
    <div>
    <div id="header">
        <header>
            <div class="stripe white-stripe">
                <h1 class="hidden">qcxxx.com</h1>
                <a href="https://www.xvideos.red" class="mobile-show" id="home-header-mobile-red" title="XVideos.RED"><span class="icon-f icf-ticket-red icf-white-fill"></span></a>
                <a href="/" class="logo" id="site-logo-link">
                <svg id="site-logo-svg" height="36" width="144">
                <image :xlink:href="vlogo" src="https://static-l3.xvideos-cdn.com/v3/img/skins/default/xvideos.com.png" title="XVideos 主頁" height="36" width="144" class="no-blur" id="site-logo"></image>
                </svg>
                </a> 
                <form action="/" id="xv-search-form" class="mobile-hide" style="max-width: 1185px;float:right;"  @submit.prevent="onSubmit">
                <div class="cont">
                <div class="input-group">
                <input id="searchInput" type="text" name="k" value="" class="search-input form-control" maxlength="2048" placeholder="Search 9,147,020 videos" autocomplete="off" ref="myh3" @focus="handleinput()" v-model="keywords"><span class="input-group-btn"><button type="submit" title="搜索" class="search-submit btn btn-default" @click="doSearch()" ><span class="mobile-show-inline-block" style="display:block;">搜索</span></button></span>
                </div>
                </div>
                </form>
                <h2 class="mobile-hide" id="dsktp-title-comment" style="visibility: visible;"><span class="pad">最佳 <span class="text-danger">免費成人影視</span>網站</span></h2>
            </div>
        </header>
    </div>
    <div class="search-autocomplete" style="min-width: 470px; max-width: 1553px; max-height: 102px; overflow-y: auto;display:block;" v-bind:style="searchStyle" v-if="_searchList">
    <ul class="keywords" style="max-height: none;">
        <router-link :to="'/listb/'+v"  v-for="v in search_history">
        <li style="padding-left:12px;padding-right:12px">
                <span class="keywords"><span class="highlighted">{{v}}</span></span>
        </li>
        </router-link>
    </ul>
    </div> 
    <div style="clear:both;"></div> 
    </div> 
</template>
<script>
import vlogo from '@/assets/logo.svg'
export default {
  name: 'Header',
  data(){
        return {
            search_history:this.cstorage.get('_search_'),
            keywords:this.$route.params.k,
            vlogo:vlogo,
            isOpen:0,
            searchList:0,
            _activeRight:0,
            _activeTop:0,
            compStyle:''
        }
        
  },
  mounted(){
        this.$on('handleinput',(val)=>{
            this.handleinput(val);
        });

  },  
    created() {
        this.searchDiv();
    },
    computed: {
        activeTop() {
            return this._activeTop             
        },
        activeRight() {
            return this._activeRight             
        },
        searchStyle() {
            return this.compStyle;
        },
        _searchList()  {
            return this.searchList
        } 
    },
  methods: {
        openMenu() {
            if(this.isOpen == 0){
                this.isOpen = 1
            }else{
                this.isOpen = 0
            }
            this.$parent.$refs.sidebar.isOpen=this.isOpen;
            this.handleinput(1)
        },
        doSearch() {
            //console.log(this.keywords)
            if(this.keywords == ''){
                return
            }
            this.commonfunc.saveSearch(this.keywords); // 本地存储搜索的内容         
            this.$router.replace('/listb/'+encodeURIComponent(this.keywords))
        },
        onSubmit(){
            return false;
        },
        handleinput(type){        //输入框获取焦点显示搜索历史记录
            if(type!=undefined&&parseInt(type) == 1){
                this.searchList = 0
                return
            } 
            /*
            this.isOpen = 0
            this.$parent.$refs.sidebar.isOpen=this.isOpen;
            */
            let searches=this.cstorage.get('_search_');

            if(searches!=undefined){
                this.search_history = searches
                this.searchList = 1                 
            }                                 
        },
        searchDiv() {
            this.$nextTick(() => {
                this._activeRight = parseFloat(this.$refs.myh3.getBoundingClientRect().right)
                this._activeTop = parseFloat(this.$refs.myh3.getBoundingClientRect().top)
                //console.log(this.$refs.myh3.getBoundingClientRect())
                this.compStyle = 'top:'+ (parseFloat(this.$refs.myh3.getBoundingClientRect().height)) + 'px;'+ 'left:'+ (parseFloat(this.$refs.myh3.getBoundingClientRect().x)-80) + 'px'
                //alert(this._activeTop)
            })
            //return {"right":"200px","top":"200px"}  
        }        
  },
  watch: {
    '$route' (to,from) {
      this.keywords = this.$route.params.k
      this.search_history=this.cstorage.get('_search_')
      this.searchList = 0
    }
  }, 
}
</script>        