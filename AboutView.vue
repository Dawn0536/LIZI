<template>
  <div class="about">
    <canvas id="canvas" ref="canvas"></canvas>
  </div>
</template>
<script setup>
import {ref,onMounted} from 'vue'
const canvas =ref("")
onMounted(()=>{
  canvas.value.width=window.innerWidth
  canvas.value.height=window.innerHeight
  const ctx =canvas.value.getContext("2d")
  class Dot {
    constructor(x,y){
      this.x=x
      this.y=y
      this.dirX=Math.random() * 10 -5
      this.dirY=Math.random() * 10 -5
      this.color=`hsl(${Math.random()*360},50%,50%)`
    }
    draw(){
      ctx.beginPath()
      ctx.fillStyle =this.color
      ctx.arc(this.x,this.y,2,0,Math.PI * 2)
      ctx.fill()
      ctx.closePath()
    }
    updata(){
      if(this.y>canvas.value.height || this.y <= 0){
        this.dirY=-this.dirY
      }
      if(this.x>canvas.value.width || this.x <= 0){
        this.dirX=-this.dirX
      }
      

      this.x += this.dirX
      this.y +=this.dirY
      this.draw()
    }
  }
  // let dots=new Dot(400,400)
  // dots.draw()

  const arr =[]
  function init(){
    for (let i=0;i<200;i++){
      let dots=new Dot(Math.random()* canvas.value.width,Math.random()* canvas.value.height)
      arr.push(dots)
    }
  }
  init()
  function animation(){
    ctx.clearRect(0,0,canvas.value.width,canvas.value.height)
    requestAnimationFrame(animation)
    arr.forEach((item,index)=>{
      arr.forEach((items,indexs)=>{
        if(index == indexs) return
        if(Math.abs(item.x - items.x ) < 110 && Math.abs(item.y - items.y ) < 110){
          item.color=`hsl(${Math.random()*360},50%,50%)`
          setLine(item.x,item.y,items.x,items.y,item.color)
        }

      })
        item.updata()
    })
  }
  function setLine(x,y,x1,y1,color){
    ctx.beginPath()
    ctx.strokeStyle=color
    ctx.moveTo(x,y)
    ctx.lineTo(x1,y1)
    ctx.stroke()
    ctx.closePath()
  }
  animation()

})
</script>
<style>
*{
  margin: 0;
  padding: 0;
}
.about{
  height: 100vh;
  overflow: hidden;
}
#canvas{
  /* background-color: #fff; */
  background-image: linear-gradient(to right, #434343 0%, black 100%);
  /* background-image: linear-gradient(to top, #a18cd1 0%, #fbc2eb 100%); */
}
</style>
