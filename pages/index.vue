<template>
  <div class="text-center h-0 font-bold text-2xl translate-y-[20vh]">Type in what you want to hear <br> </div>
    <div class="bg-gradient-to-tr from-blue-500 to-blue-300 border-10 rounded-3xl container mx-auto max-w-xl translate-y-[25vh]">
      <div class="py-5 px-10">
        <!-- <h1 class="text-center font-bold text-2xl">Type in what you want to hear</h1> <br> -->
        <input id="text" class="p-5 min-w-full text-center border-[5px] rounded-xl bg-blue-300 border-blue-300 focus:border-blue-100 focus:outline-none placeholder:text-gray-500" type="text" placeholder="Say something!" v-model="text"><br>
        <div class="text-center font-medium h-0 text-xl translate-y-[1vh]">Choose your Preference</div>
        <div class="flex justify-center space-x-4">
          <div>
            <select name="lang" id="inLang" class="mt-10 mx-auto border-2">
              <option value="en-US">EN</option>
              <option value="id-ID">ID</option>
              <option value="es-ES">ES</option>
              <option value="ja-JP">JP</option>
            </select>

          </div>
          <div>
            <h2 class="mt-10 font-semibold">TO</h2>
          </div>
          <div>
            <select name="toLang" id="toLang" class="mt-10 mx-auto border-2">
              <option value="en-US">EN</option>
              <option value="id-ID">ID</option>
              <option value="es-ES">ES</option>
              <option value="ja-JP">JP</option>
            </select>
          </div>
        </div><br>
        <div class="flex justify-center space-x-4 pt-2">
          <button type="submit" @click="makeSound()" class=" bg-stone-100 shadow-2xl border-[5px] rounded-xl  font-semibold">Speak!</button>
          <button type="submit" @click="translateSpeak()" class="bg-stone-100 border-[2px] rounded-xl font-semibold">Translate and Speak!</button>
          <button type="submit" @click="translateText()" class="bg-stone-100 shadow-2xl border-[5px] rounded-xl font-semibold">Translate!</button>
        </div>
        <!-- <div class="flex justify-center pt-5">
          <button type="submit" @click="translateSpeak()" class="bg-stone-100 border-[2px] rounded-xl font-semibold">Translate and Speak!</button>
        </div> -->
        <section id="translate" :class="{ 'translate-y-[1vh]': true, 'bg-stone-100': viewTranslate, 'shadow-2xl': viewTranslate, 'border-[2px]': viewTranslate, 'rounded-xl': true, 'text-center': true }"></section>
        <section id="warning" :class="{ 'translate-y-[1vh]': true, 'bg-stone-100': viewWarning, 'shadow-2xl': viewWarning, 'border-[2px]': viewWarning, 'rounded-xl': true, 'text-center': true }"></section>
      </div>
    </div>
</template>

<script>

//import Speech from 'speak-tts'
import translate from 'translate'
import fs from 'fs'

  export default {
    data() {
      return {
        viewTranslate: false,
        viewWarning: false,
        text: '',
      }
    },
    methods: {
      async makeSound() { 

        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''
        // say.speak({
        //   text: this.text
        // })
        // say.speak({
        //   text: this.text
        // })
        const utterance = new SpeechSynthesisUtterance();
        // console.log(this.text)
        utterance.text = this.text
        utterance.lang = document.querySelector('#inLang').value
        window.speechSynthesis.speak(utterance);
      },
      async translateText() {
        // const toLang = document.querySelector("#toLang").value
        // console.log(toLang)
        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''

        // if (this.text == '')

        const fromLang = (document.querySelector("#inLang").value).slice(0, -3)
        const toLang = (document.querySelector("#toLang").value).slice(0, -3)
        if (fromLang === toLang)
        {
          this.viewWarning = true
          document.querySelector('#warning').innerHTML = 'The language in which you are trying to translate and the destination language is identical'
        }

        else
        {
          const trans = await translate(this.text, { from: fromLang, to: toLang })
          console.log(trans)
          document.querySelector('#translate').innerHTML = trans
        }
        // const trans = await translate(this.text, toLang)
        // document.querySelector('#translate').innerHTML = trans
      },
      async translateSpeak() {
        this.viewWarning = false
       
        
        document.querySelector('#warning').innerHTML = ''
        document.querySelector('#translate').innerHTML = ''

        const utterance = new SpeechSynthesisUtterance();

        // const fromLang = document.querySelector("#lang").value
        const fromLang = document.querySelector("#inLang").value
        const toLang = document.querySelector("#toLang").value
        // let fromLangv2 = ''
        // let toLangv2 = ''

        // if (fromLang != 'en') {
        //   let fromLangv2 = fromLang +"-"+ fromLang.toUpperCase()
        //   console.log(fromLangv2)
        // }
        // else
        // {
        //   fromLangv2 = 'en-US'
        // }
        console.log(fromLang, toLang)
        
        let fromLangv2 = fromLang.slice(0, -3)
        let toLangv2 = toLang.slice(0, -3)
        
        // if (toLang != 'en') {
        //   toLangv2 = toLang +"-"+ toLang.toUpperCase()
        //   console.log(toLangv2)
        // }
        // else
        // {
        //   toLangv2 = 'en-US'
        // }

        console.log(fromLangv2, toLangv2)

        // translate.from = fromLang
        console.log(fromLangv2, toLangv2)
        // if (this.text == '')
        if (fromLang === toLang)
        {
          this.viewWarning = true
          this.viewTranslate = false
          document.querySelector('#warning').innerHTML = 'The language in which you are trying to translate and the destination language is identical'
        }
        else
        {
          this.viewTranslate = true
          if (this.text == '')
          {
            document.querySelector('#translate').innerHTML = ''
          }
  
          const trans = await translate(this.text, { from: fromLangv2, to: toLangv2 })
  
          document.querySelector('#translate').innerHTML = trans
  
          console.log(trans)
  
          utterance.text = trans
  
          utterance.lang = toLang
  
          window.speechSynthesis.speak(utterance);
        }
        

        // const trans = await translate(this.text, { from: fromLang, to: toLang })

        // document.querySelector('#translate').innerHTML = trans

        // console.log(trans)

        // utterance.text = trans

        // utterance.lang = toLangv2

        // window.speechSynthesis.speak(utterance);
      }
    },
  }
</script>
