<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref,onMounted } from 'vue'

class graph{
  points:any
  constructor(pointNum:number,maxDia=1000){
    this.points =new Array(pointNum).fill(0).map(()=>new point(Random(3,innerWidth-3),Random(3,innerHeight-3),6))
  }

  draw(ctx:CanvasRenderingContext2D){
    ctx.clearRect(0,0,innerWidth,innerHeight);
    this.points.forEach((point)=>{point.draw(ctx)})
    for(let i=0;i<this.points.length-1;i++){
    for(let j=i+1;j<this.points.length;j++){
      ctx.beginPath()
      ctx.moveTo(this.points[i].x,this.points[i].y)
      ctx.lineTo(this.points[j].x,this.points[j].y)
      ctx.lineWidth=5
      ctx.strokeStyle=`rgba(${Random(0,255)},${Random(0,255)},${Random(0,255)},${1-((Math.sqrt((this.points[i].x-this.points[j].x)**2+(this.points[i].y-this.points[j].y)**2))/1600)})`
      ctx.stroke()
    }
  }
  }

  animate(ctx:CanvasRenderingContext2D){
    this.draw(ctx);
    requestAnimationFrame(()=>{this.animate(ctx)})
    
  }
}

class point{
  x:number
  y:number
  r:number
  xSpeed:number
  ySpeed:number
  lastDrawTime:any
  constructor(x:number,y:number,r:number,xSpeed:number,ySpeed:number){
    this.x=x
    this.y=y
    this.r=r
    this.xSpeed=Random(0,50)
    this.ySpeed=Random(0,50)
    this.lastDrawTime =null
  }

  draw(ctx:CanvasRenderingContext2D){
    if(this.lastDrawTime){
      const duration = (Date.now() - this.lastDrawTime)/1000
      if(this.x>=innerWidth-10*this.r||this.x<=this.r/2){
        this.xSpeed = -this.xSpeed
      }
      if(this.y>=innerHeight-this.r/2||this.y<=this.r/2){
        this.ySpeed = -this.ySpeed
      } 
      this.x = this.x+duration*this.xSpeed
      this.y = this.y+duration*this.ySpeed

    }
    ctx.beginPath()
    ctx.arc(this.x,this.y,this.r,0,Math.PI*2)
    ctx.closePath()
    ctx.fillStyle='white'
    ctx.fill()
    this.lastDrawTime = Date.now()
  }

}


function Random(min:number,max:number){
  return Math.floor(Math.random()*(max-min+1)+min)

}


function init(){
  const cvs = document.querySelector('canvas') as HTMLCanvasElement
  const ctx = cvs.getContext('2d')
  cvs.width = window.innerWidth
  cvs.height = window.innerHeight
  const points = new graph(50)
  points.animate(ctx)

}



const print=(ctx:CanvasRenderingContext2D)=>{
  ctx.beginPath()
  ctx.moveTo(100,100)
  ctx.lineTo(400,100)
  ctx.lineTo(400,500)
  ctx.closePath()
  ctx.strokeStyle='#fff'
  ctx.stroke()
  ctx.fillStyle='white'
  ctx.fill()
 

}

onMounted(()=>{
  init()
 
})





</script>

<template>
  <div>
    <canvas id="qwq" ></canvas>
  </div>
</template>

<style>

#qwq{
  border: 1px solid black;
  background-color: black;
}
*{
  margin: 0;
  padding: 0;
}

</style>