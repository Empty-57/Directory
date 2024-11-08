<script>
export default {
  data(){
    return{
       directory_flag:false
    }
  },
  methods: {
    anchor_highlight(anchor){
      if(!anchor){
        return;
      }
          const anchor_list=document.querySelectorAll('a.anchor_hl')
          if(anchor_list){
            anchor_list.forEach(item=>{
            item.classList.remove('anchor_hl')
          })
          }
          anchor.classList.add('anchor_hl');
        }
  },
  mounted() {
    let count = 0;
    const timer3 = setInterval(() => {
      const article_a = document.querySelectorAll('h1,h2')
      if (article_a) {
        count = 0;
        const directory = document.querySelector('#directory')
        article_a.forEach((item) => {
          item.id = item.innerText.replace(' ','');
          item.className = 'anchors'
        })
        window.addEventListener('scroll',debounce(()=>{
          let hl_link;
          for (let i of article_a) {
            let top=i.getBoundingClientRect().top;
            if (top>=0 && top <=300){
              hl_link =document.querySelector('[href="#'+i.id+'"]');
              if(hl_link){
                this.anchor_highlight(hl_link)
              }
              break;
            }
          }
        },100))
        const renderFn = () => h('div', {class: 'gap-2 bg-translate h-fit p-4 pt-2 flex flex-col *:duration-200'}, Array.from({length: article_a.length}).map((item, index) => {
          return h('a', {
            class: (article_a[index].tagName === 'H1' ? '' : 'ml-4 ') + 'hover:text-cyan-600 active:text-cyan-700 text-left text-pretty break-all',
            href: "#" + article_a[index].id
          }, article_a[index].innerText)
        }))
        render(renderFn(), directory)
        this.directory_flag = true
        this.anchor_highlight(document.querySelector('[href="#'+article_a[0].id+'"]'))

        clearInterval(timer3)
      } else {
        count++
        if (count === 5) {
          clearInterval(timer3);
        }
      }
    }, 500)
  },
}
</script>

<template>
    <div id="directory" class="sticky top-16 min-w-60 w-fit opacity-0 duration-500 bg-zinc-300/60 dark:bg-zinc-900/60 rounded shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 overflow-y-auto overflow-x-hidden max-h-[80svh] self-start mx-4 hidden lg:block" :class="{'opacity-100':directory_flag}">
      <div class="sticky top-0 bg-zinc-300 dark:bg-zinc-900 w-full self-start p-4 py-2 border-b-[1px] border-zinc-700">目录</div>
    </div>
</template>
<style>
  .anchor_hl{
  color: rgb(14 116 144) !important;
  border-left-width: 2px !important;
  border-color: rgb(14 116 144) !important;
  padding: 0.5rem !important;
  cursor: default !important;
  pointer-events: none !important;
}
.anchor_hl:hover{
  color: rgb(14 116 144) !important;
}
</style>
