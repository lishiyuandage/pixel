<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- 透明-->
    <button class="btn">选择文件</button>
    <input type="file" name="img" accept="image/*" class="btn" style="opacity:0;"  @change="handleFileChange"/>
    <select v-model="scale" @change="handleScaleChange">
      <option v-for="key of scales"  :key="key" :value="key">{{Math.pow(key,2)}}</option>
    </select>
    <div>{{filename}}</div>
    <canvas id="pixel"></canvas>
  </div>
</template>

<script>
export default {
  name: 'Home',
  props: {
    msg: String
  },
  data() {
    return {
      scales: [1,50,100,200,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20],
      filename: '',
      scale: 1,
      fileUrl:''
    }
  },
  methods:{
    handleScaleChange(){
      let img = new Image()
      img.src = this.fileUrl;
      img.onload = () => {
        document.getElementById('pixel').width = img.width
        document.getElementById('pixel').height = img.height
        let ctx = document.getElementById('pixel').getContext('2d')
        ctx.drawImage(img, 0, 0)
        let getImageData = ctx.getImageData(0,0,img.width,img.height)
        // 四个一组，每一组代表一个像素，每一组四个值分别代表r,g,b,a
        let data = getImageData.data
        let height = img.height
        let width = img.width 
        let scale = this.scale
        let newData = []
        let widthPix = width * 4
        for(let i = 0; i < data.length; i += 4){
          let p = i / 4
          // 如果不是第scale行，则复制上一行的像素
          if(Math.floor(p / width) % scale != 0){
            newData.push(newData[i-widthPix])
            newData.push(newData[i-widthPix+1])
            newData.push(newData[i-widthPix+2])
            newData.push(newData[i-widthPix+3])
          }
          // 如果不是第scale列，则复制上一列的像素
          else if( p  % scale != 0){
            newData.push(newData[i-4])
            newData.push(newData[i-3])
            newData.push(newData[i-2])
            newData.push(newData[i-1])
          }
          // 如果是第scale行，第scale列，则使用当前像素
          else{
            newData.push(data[i])
            newData.push(data[i+1])
            newData.push(data[i+2])
            newData.push(data[i+3])
          }
        }
        let newImageData = new ImageData(new Uint8ClampedArray(newData), width , height )
        // 填充新的像素
        ctx.putImageData(newImageData, 0, 0)
      }
    },
    handleFileChange(e){
      this.filename = e.target.files[0].name
      let file = e.target.files[0]
      let reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = (e) => {
        this.fileUrl = e.target.result
        this.handleScaleChange()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
.btn{
  /* Auto layout */
  justify-content: center;
  align-items: center;
  padding: 12px 16px;
  position: absolute;
  height: 48px;
  left: 22px;
  top: 152px;

  background: rgba(253, 171, 171, 0.5);
  border: 2px solid rgba(243, 62, 62, 0.7);
  box-sizing: border-box;
  border-radius: 48px;
  /* 订单历史 */
  font-family: 'Inter';
  font-style: normal;
  font-weight: 400;
  font-size: 20px;
  line-height: 24px;
  display: flex;

  color: #F30000;
  
}
</style>
