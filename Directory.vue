<script setup>
import {h, onMounted, ref, render} from "vue";
import {debounce} from "@/assets/js/debounce.js";
import {Expo, gsap} from "gsap";
import {ScrollToPlugin} from "gsap/ScrollToPlugin";

gsap.registerPlugin(ScrollToPlugin);
const directory_flag = ref(false)

function anchor_highlight(anchor) {
  if (!anchor) {
    return;
  }
  const anchor_list = document.querySelectorAll('span.anchor_hl')
  if (anchor_list) {
    anchor_list.forEach(item => {
      item.classList.remove('anchor_hl')
    })
  }
  anchor.classList.add('anchor_hl');
}

onMounted(() => {
  let count = 0;
  const timer = setInterval(() => {
    const article_a = document.querySelectorAll('h1,h2')
    if (article_a) {
      count = 0;
      const directory = document.querySelector('#directory')
      article_a.forEach((item) => {
        item.id = item.innerText.replace(' ', '');
        item.className = 'anchors'
      })
      window.addEventListener('scroll', debounce(() => {
        let hl_link;
        for (let i = 0; i < article_a.length; i++) {
          let top = article_a[i].getBoundingClientRect().top;
          if (top >= 0 && top <= 300) {
            hl_link = document.querySelector('[id="#' + article_a[i].id + '"]');
            anchor_highlight(hl_link)
            break;
          }
          if (top < 0 && article_a[i + 1] && article_a[i + 1].getBoundingClientRect().top > document.documentElement.clientHeight) {
            hl_link = document.querySelector('[id="#' + article_a[i].id + '"]');
            anchor_highlight(hl_link)
            break;
          }
          if (top < 0 && article_a[i + 1] && article_a[i + 1].getBoundingClientRect().top < document.documentElement.clientHeight && article_a[i + 1].getBoundingClientRect().top > 300) {
            hl_link = document.querySelector('[id="#' + article_a[i].id + '"]');
            anchor_highlight(hl_link)
            break;
          }
          if (top < 0 && !article_a[i + 1]) {
            hl_link = document.querySelector('[id="#' + article_a[i].id + '"]');
            anchor_highlight(hl_link)
            break;
          }
        }
      }, 100))
      const renderFn = () => h('div', {
        id: 'anchor_box',
        class: 'gap-2 bg-translate h-fit p-4 pt-2 flex flex-col *:duration-200'
      }, Array.from({length: article_a.length}).map((item, index) => {
        return h('span', {
          class: (article_a[index].tagName === 'H1' ? '' : 'ml-4 ') + 'hover:text-cyan-600 active:text-cyan-700 text-left text-pretty break-all',
          id: "#" + article_a[index].id
        }, article_a[index].innerText)
      }))
      render(renderFn(), directory)
      directory_flag.value = true
      const anchors = document.querySelector('#anchor_box').childNodes
      anchors.forEach(item => {
        item.onclick = e => {
          const to_elem = document.querySelector('[id="' + e.target.id.split('#')[1] + '"]')
          gsap.to(window, {duration: 0.5, scrollTo: to_elem.offsetTop - 50, ease: Expo.easeOut})
        }
      })
      anchor_highlight(document.querySelector('[id="#' + article_a[0].id + '"]'))

      clearInterval(timer)
    } else {
      count++
      if (count === 5) {
        clearInterval(timer);
      }
    }
  }, 500)
})
</script>

<template>
  <div id="directory"
       :class="{'opacity-100':directory_flag}"
       class="*:text-sm sticky top-16 min-w-60 w-fit opacity-0 duration-500 bg-zinc-300/60 dark:bg-zinc-900/60 rounded shadow shadow-zinc-400/60 dark:shadow-zinc-950/60 overflow-y-auto overflow-x-hidden max-h-[80svh] self-start mx-4 hidden lg:block">
    <div class="sticky top-0 bg-zinc-300 dark:bg-zinc-900 w-full self-start p-4 py-2 border-b-[1px] border-zinc-700">
      目录
    </div>
  </div>
</template>
<style>
.anchor_hl {
  color: rgb(14 116 144) !important;
  border-left-width: 2px !important;
  border-color: rgb(14 116 144) !important;
  padding: 0.5rem !important;
  cursor: default !important;
  pointer-events: none !important;
}

.anchor_hl:hover {
  color: rgb(14 116 144) !important;
}
</style>
