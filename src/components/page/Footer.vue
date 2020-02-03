<template>
<div>
<!--广告后期开放-->
<div id="ad-footer" class=" ad-footer ad-support-desktop" v-if="ads1Data">
  <a :href="ads1Data.url" target="_blank" rel="noopener"><img :src="ads1Data.vimg"></a>
</div>
<div id="nav-trends" class="nav-elem">
<div class="home-trends ordered-label-list"><nav aria-label="secondary">
<ul>
<li  v-for="site in trData" >
<router-link :to="'/listt/'+site._id" class="btn btn-default">{{site.title}}
</router-link> 
</li>
</ul></nav></div>
</div>
<div class="remove-ads" v-if="ads2Data">
    <p>
  <a :href="ads2Data.url" target="_blank" rel="noopener"><img :src="ads2Data.vimg"></a>
    </p>
</div>
<div id="footer">
    <footer>
        <!--
        <div class="terms">
            <p>
                XVideos.com is a <strong>free hosting service for porn videos</strong>.
                We convert your files to various formats.
                You can grab our 'embed code' to display any video on another website.
                Every video uploaded, is shown on our indexes more or less three days after uploading.
                About 1200 to 2000 adult videos are uploaded each day (note that gay and shemale videos are filtered from this page, but shown in their respective categories).
                Our pages (everything that you see hosted on www.xvideos.com) contain absolutely no spyware/adware/trojan/etc.
                There is no charge (no hidden charges either) for viewing our videos.
            </p>
            <p class="newp">
                <em>XVIDEOS</em> is rated with RTA label. Parents, you can easily block access to this site.
                Please <a href="http://www.rtalabel.org/index.php?content=parents/">read this page</a> for more informations.
            </p>
            <div class="botLinks"><a href="https://info.xvideos.com/legal/tos/">服务条款</a> -
                <a href="https://info.xvideos.com/">上傳您的視頻</a> - <a href="https://info.xvideos.com/content/">刪除內容</a> -
                <a href="https://main.trafficfactory.biz/xvideos-signup">廣告投放</a> -
                <a href="https://twitter.com/xvideoscom" target="_blank" rel="noopener, noreferrer">XVideos on Twitter</a> -
                <a href="https://www.xvideos.com/rss/del.xml">RSS 刪除</a> - <a href="http://www.xvideos.net/app">XVideos Android App</a> -
                <a href="https://www.xvideos.red/?pmln=zh&amp;pmth=light&amp;pmsc=footer">XVIDEOS RED</a> - <a href="https://info.xvideos.com/">更多選項</a></div>
        </div>
        -->
        <p class="slogan">qcxxx.com - 網絡上的最佳成人影視，100%免費</p>
    </footer>
</div>
</div>
</template>
<script>
export default {
  name: 'Footer',
    data() {
        return {
            trendsUrl: this.GLOBAL.serviceUrl+'?t=9',
            adsUrl: this.GLOBAL.serviceUrl+'?t=12',
            trendsData:[],
            ads1:{},
            ads2:{},
        }
    },
    created() {
        this.getTrendsData();
        this.getAdsData();
        this.getAdsData(1);
    },
    computed: {
        trData() {
            return this.trendsData
        },
        ads1Data() {
            return this.ads1
        },
        ads2Data() {
            return this.ads2
        },
    },
    watch: {
      '$route' (to,from) {
        this.more_load = 0
        this.getAdsData();
        this.getAdsData(1);
      }
    },
    methods: {
        getTrendsData() {
            var a=this.commonfunc._getRandom()
            var nonce = parseInt(a)
            var token=this.commonfunc._gtokene(nonce)
            var that = this  
            this.axios.post(this.trendsUrl,this.qs.stringify({a: 1}),{
                    headers: {'Content-Type':'application/x-www-form-urlencoded','nonce':nonce,'token':token}
                }).then(function(res){
                if(parseInt(res.data.message)==0){                  
                   that.trendsData = res.data.data;   
                }
                that.loadTag = 0       
            }).catch(function(err){
             console.log(err)
            })                             
        },
        getAdsData(_type) {
            var a=this.commonfunc._getRandom()
            var nonce = parseInt(a)
            var token=this.commonfunc._gtokene(nonce)
            var that = this  
            this.axios.post(this.adsUrl,this.qs.stringify({a: 1}),{
                    headers: {'Content-Type':'application/x-www-form-urlencoded','nonce':nonce,'token':token}
                }).then(function(res){
                if(parseInt(res.data.message)==0){
                   if(_type){
                        that.ads1 = res.data.data;   
                   }else{
                        that.ads2 = res.data.data;  
                   }                  
                     
                }
                that.loadTag = 0       
            }).catch(function(err){
             console.log(err)
            })                             
        },
    }
}
</script>        