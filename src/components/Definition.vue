<template>

<div class = "definitions">
    <div v-if="!props.word" class = "tip">
        Start by typing... 
    </div>
    <div v-else-if="props.word && !result.length" class = "tip">
        Sorry, The definition of the vocabulary is not found
    </div>
   <div v-else
        class = "definition" 
        v-for="(definition, index) in result" 
        :key="index">
        <div class="phonetics">
            <span class = "phonetic"  @click="handleClick(index)">
            {{ definition.phonetics[0].text }}
            </span>
            <span></span>
            <i class="iconfont" @click="handleClick(index)">&#xe600;</i>
            <audio :src="definition.phonetics[0].audio"  ref="audio" ></audio>
            <div class="meanings">
                <div class="meaning" v-for="(meaning, index) in definition.meanings" :key="index">
                    <span class ="partOfSpeech"> {{ meaning.partOfSpeech }}</span>
                    <br />
                        <template v-for="(defi, index_) in  meaning.definitions" :key="index_">
                            <span class = "word-meaning">{{ "(" +  (index_ + 1 ) + ") " + defi.definition }}</span>
                            <br />

                            <template v-if="defi.example">
                            <!-- {{ defi.example }} -->
                                <span class = "word-example" v-html="highLight(defi.example)"></span>
                            </template>
                            <template v-else >
                                <span class = "word-example">Example: None.</span>
                            </template>
                            <br />

                            <template v-if="defi.antonyms.length > 0">
                                <span class = "word-example">{{ "Antonyms: " + defi.antonyms.map(antonyms => antonyms).join(' | ') }}</span>
                            </template>
                            <template v-else >
                                <span class = "word-example">Antonyms: None.</span>
                            </template>
                            <br />

                            <template v-if="defi.synonyms.length > 0">
                                <span class = "word-example">{{ "Synonyms: " +  defi.synonyms.map(synonyms=>synonyms).join(' | ') }}</span>
                            </template>
                            <template v-else >
                                <span class = "word-example">Synonyms: None</span>
                            </template>
                            <br />
                            <!-- <span>{{ defi.example }}</span> -->
                            
                        </template>
                   
                </div>
            </div>
            
        </div>
    </div> 
</div>
</template>


<script setup>

import axios from "axios"
import  { watch, ref } from 'vue'

const props = defineProps({
  word : String,
});


watch(() => props.word, debounce)
const result = ref([]);

function highLight(title){
    var res = title;
      // 如果标题中包含，关键字就替换一下
    if(res.includes(props.word)){
        res = res.replace(
            props.word,
            // 这里是替换成html格式的数据，最好再加一个样式权重，保险一点
            '<font style="background-color: #ff6600;">'+ props.word +'</font>'
        )
        return res
    }
    // 不包含的话还用这个
    else{
        return title
    }
}

function handleClick(index){
    audio.value[index].currentTime = 0;
    audio.value[index].play()

}

const audio = ref()

let timeout;
let context = this
function debounce(newValue){
  clearTimeout(timeout);
  timeout = setTimeout(function(){
    getData(newValue).apply(context);
  },500)
}
async function getData(newValue){
  const url =  `https://api.dictionaryapi.dev/api/v2/entries/en/${newValue}`;
  console.log(url)
  result.value = []
  try{
    const data = await axios.get(url);
    result.value = data.data;
    console.log(result.value)
  }catch(err){
  }

}

</script>
<style lang="scss" scoped>
::-webkit-scrollbar{
    width: 5px;
    background: #282c34;
}

::-webkit-scrollbar-thumb{
    background: rgba(255,255,255,0.6);
    border-radius : 20px;
}
.definitions{
    border: 13px solid #696969;
    border-radius: 15px;
    height: 65xh;
    padding: 30px;
    overflow-y: scroll;
    display:flex;
    flex-direction: column;
    gap:5px;
    margin: 15px;
    .tip{
        font-size:18px;
    }
    .definition{
        margin-bottom: 10px;
        .phonetics{
            margin-bottom:10px;
            .phonetic{
                cursor: pointer;
                border-radius: 5px;
                &.hover{
                    opacity: 0.8;
                }
            }
            .iconfont{
                cursor: pointer;
                &:hover{
                    opacity: 0.8;
                }
            }
        }
        
        .meanings{
            display:flex;
            flex-direction: column;
            gap:10px;
            padding-bottom: 10px;
            // border-bottom: 1px solid #c5bcbc;
            div.meaning {
                .partOfSpeech{
                    margin-right: 10px;
                    background-color: #3b3e56;
                    // background-color: red;
                    opacity: 0.6;
                    display: inline-block;
                    padding-block: 3px;
                    padding-inline : 5px;
                    font-size: 26px;
                    border-radius: 5px;
                    line-height: 35px;
                    
                }
                .word-meaning{
                    line-height: 35px;
                    font-size: 22px;
                    // 
                }
                .word-example{
                    display:block;
                    text-indent:2em;
                    line-height: 18px;
                    font-size: 18px;
                    opacity: 0.5;
                    margin-bottom:0;
                    // line-height: 20px;
                    // font-size:18px;
                    // margin-bottom: 3px;
                    // font-weight:bolder;
                }
                border-bottom: 1px solid #c5bcbc;
            }
        }
        &:last-child{
            .meanings{
                border-bottom: none;
            }
        }
    }
}
</style>