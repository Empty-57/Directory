<script>
export default {
  data(){
    return{
       directory_flag:false
    }
  },
  mounted() {
    let count = 0;
    const timer3 = setInterval(() => {
      if(document.querySelector('#directory').firstChild){
        count=0;

        const directory=document.querySelector('#directory')
        const article_a=document.querySelectorAll('h1,h2')
        article_a.forEach((item) => {
          item.id=item.innerText
        })
        const renderFn = () => h('div',{class:'gap-2 bg-translate h-fit p-4 flex flex-col *:duration-200'},Array.from({length:article_a.length}).map((item,index)=>{
          return h('a',{class:(article_a[index].tagName==='H1'? '':'ml-4 ')+'hover:text-cyan-600 active:text-cyan-700 text-left text-pretty break-all',href:"#"+article_a[index].id},article_a[index].innerText)
        }))
        render(renderFn(),directory)
        this.directory_flag=true

        clearInterval(timer3)
      }else {
        count++
        if (count === 5) {
          clearInterval(timer3);
        }
      }
    },500)
  },
}
</script>

<template>
  <HomeMenu></HomeMenu>
  <ToTop></ToTop>
    <div id="directory" class="sticky top-16 min-w-60 w-fit opacity-0 duration-500 bg-zinc-300/60 dark:bg-zinc-900/60 rounded shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 overflow-y-auto overflow-x-hidden max-h-[80svh] self-start mx-4 hidden lg:block" :class="{'opacity-100':directory_flag}">
      <div class="sticky top-0 bg-zinc-300 dark:bg-zinc-900 w-full self-start p-4 py-2 border-b-[1px] border-zinc-700">目录</div>
    </div>

</template>
