<script>
export default {
  mounted() {
    let count = 0;
    const timer3 = setInterval(() => {
      if(document.querySelector('#article_body').firstChild){
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
    <div id="directory" class="bg-zinc-300/60 dark:bg-zinc-900/60 rounded shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 overflow-y-auto overflow-x-hidden max-h-[80svh] self-start sticky top-16 mx-4 hidden lg:block">
      <div class="sticky top-0 bg-zinc-300 dark:bg-zinc-900 w-full self-start p-4 py-2 border-b-[1px] border-zinc-700">目录</div>
  </div>

</template>
