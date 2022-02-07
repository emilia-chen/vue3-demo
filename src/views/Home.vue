<template>
  <div class="home">
    <Header :has_back="false" />

 <!--    {{test2()}} -->
 <div>
   obj:{{obj}}
 obj1:{{obj1}}
  obj2:{{obj2}}
   obj3:{{obj3}}
    obj4:{{obj4}}
 </div>
 <div @click='ceshi'>ceshi</div>
    <Product :items="likes" @inquire="inquire" />
    <Product :items="recommends" @inquire="inquire" />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import { ref,reactive, onMounted,toRefs,getCurrentInstance } from "vue";
import Product from "../components/Product";
import { useRouter } from "vue-router";
import { post } from "../util/tool";

export default {
  components: {
    Header,
    Product
  },
  setup(props,context) {
     const instance = getCurrentInstance() // 有效
    const { likes, recommends } = useData();
    const { inquire } = useDetailPage();
    let obj = ref({
      a:1
    });
     let obj1 = reactive({
      b:1
    });
     let obj2 = ref(1);
   let obj3 = reactive(1);

let r = reactive({a:1});
    console.log(r);
    let s = r.a;
    //let s = toRef(r, 'a');
    console.log(s);
     // let obj4 = toRefs(obj1)

    // test()
    // function test2(){
    //   console.log('test2');
    //   return 2;
    // }
   //test2()
   function ceshi(){
  console.log('ceshi',instance);
    console.log('obj',obj,'obj1',obj1,'obj2',obj2,'obj3',obj3,'obj4',obj4);
    obj.value.a++;
    obj1.b++;

obj2.value++;
obj3++;
//obj4.b++;
 // instance.ctx.likes.data[0].price=3000
 //  instance.ctx.likes.data[0].unit='个'
  return 1;
}

    return {
       r,s,
    //  ...obj4,
      obj,
      obj1,
      obj2,
      obj3,
      likes,
      recommends,
      inquire,
      ceshi
      // test2,
      // test
    };
  }
};



// function ceshi(){

//    const  ctx=getCurrentInstance()
//      console.log('ceshi',ctx);
//   ctx.likes.value.data[0].price=3000
// }


/**
 * 进入详情页面相关操作
 */
function useDetailPage() {
  const router = useRouter();
  function inquire(data) {
    const { id } = data;
    router.push(`/detail/${id}`);
  }
  return {
    inquire
  };
}

function useData() {
  let state=reactive({
    likes:{},
    recommends:{}
  })
  let likes = ref({});
 let recommends = ref({});
 console.log('likes',likes)
  onMounted(() => {
    init();
  });

  function init() {
    return new Promise(resolve => {
      post({
        url: "/api/home"
      }).then(result => {
       console.log('result',result)
      likes.value = result.likes;
      recommends.value = result.recommends;
       
       state.likes = result.likes;
       state.recommends = result.recommends;
    // likes=reactive(result.like);
    // recommends=reactive(result.recommends);

   console.log('likes1', toRefs(state))
      });
    });
  }

  return {
    ...toRefs(state),
   // likes,
   // recommends
  };
}
</script>
