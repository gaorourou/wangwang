<template>
  <div class="movie_body">
    <meta name="referrer" content="never">
    <ul>
      <!-- <li>
        <div class="pic_show"><img src="/images/movie_1.jpg"></div>
        <div class="info_list">
          <h2>无名之辈</h2>
          <p><span class="person">17746</span> 人想看</p>
          <p>主演: 陈建斌,任素汐,潘斌龙</p>
          <p>2018-11-30上映</p>
        </div>
        <div class="btn_pre">
          预售
        </div>
      </li> -->
      <!-- <li v-for="item in comingList" :key="item.id">
        <div class="pic_show"><img :src="item.img | setWH('128.180')"></div>
        <div class="info_list">
          <h2>{{item.nm}}</h2>
          <p><span class="person">{{item.wish}}</span> 人想看</p><br>
          <div style="display:inline">主演:</div><p>{{item.star}}</p><br>
          <p>{{item.showInfo}} 上映</p>
        </div>
        <div class="btn_pre">
          预售
        </div>
      </li> -->
      <li v-for="item in movieList" :key="item.id">
        <div class="pic_show"><img :src="item.images.small"></div>
        <div class="info_list">
          <h2>{{item.title}}</h2>
          <p><span class="person">{{item.collect_count}}</span> 人想看</p><br>
          <div style="display:inline">主演:</div><p v-for="(cast,index) in item.casts" :key="index">{{cast.name+","}}</p><br>
          <p>{{item.mainland_pubdate}} 上映</p>
        </div>
        <div class="btn_pre">
          预售
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name:"ComingSoon",
  data(){
    return {
     movieList : [],
     name : ""
    }
  },
  // mounted(){
  //   this.axios.get('/api/movieOnInfoList?cityId=10')
  //   .then((res)=>{
  //     var msg = res.data.msg;
  //     if(msg === "ok"){
  //       var movieList = res.data.data.movieList;
  //       for(var i=0;i<movieList.length;i++){
  //         if(movieList[i].sc==0){
  //           this.comingList.push(movieList[i]);
  //         }
  //       }
  //     }
  //   })
  // }
  mounted(){
    this.axios.get('https://douban.uieee.com/v2/movie/coming_soon')
    .then((res)=>{
      console.log(res);
      var statusText = res.statusText;
      console.log(statusText);
      if(statusText === "OK"){
        this.movieList = res.data.subjects;
        console.log(this.movieList);
      }
    })
  }
}
</script>
<style scoped>
.movie_body ul{ margin:0 12px; overflow: hidden;margin-top: 95px;}
.movie_body ul li{ margin-top:12px; display: flex; align-items:center; border-bottom: 1px #e6e6e6 solid; padding-bottom: 10px;}
.movie_body .pic_show{ width:64px; height: 90px;}
.movie_body .pic_show img{ width:100%;}
.movie_body .info_list { margin-left: 10px; flex:1; position: relative;}
.movie_body .info_list h2{ font-size: 17px; line-height: 24px; width:150px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;}
.movie_body .info_list p{ font-size: 13px; color:#666; line-height: 22px; width:200px; overflow: hidden; white-space: nowrap; text-overflow:ellipsis;display: inline;}
.movie_body .info_list .grade{ font-weight: 700; color: #faaf00; font-size: 15px;}
.movie_body .info_list img{ width:50px; position: absolute; right:10px; top: 5px;}
.movie_body .btn_mall , .movie_body .btn_pre{ width:47px; height:27px; line-height: 28px; text-align: center; background-color: #f03d37; color: #fff; border-radius: 4px; font-size: 12px; cursor: pointer;}
.movie_body .btn_pre{ background-color: #3c9fe6;}
</style>
