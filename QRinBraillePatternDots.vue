<template>
  <div style="display: inline-block;line-height: 1;font-weight:bolder;letter-spacing:-3px;color:#000;text-shadow:0 0 3px #000;background:#fff;white-space:pre-wrap;outline:black 1px;-webkit-text-stroke-color: black;-webkit-text-stroke:.07em">{{code}}</div>
</template>
<script>
  import qr from 'qr.js'
  export default {
    props: {
      value: String,
    },
    computed: {
      code(){
        const qrcode = qr(this.value)
        const cells = qrcode.modules
        const space = "   \n   "
        let cliQr = space
        for (let r = 0; r < cells.length; r+=4 ) {
          for (let c = 0; c < cells[0].length; c+=2 ) {
            let numBlock = 10240                              // blank and start
            if (cells[r][c])                   numBlock += 1  // 1
            if (cells[r+1] && cells[r+1][c])   numBlock += 2  // 2   
            if (cells[r+2] && cells[r+2][c])   numBlock += 4  // 3    1 4
            if (cells[r]   && cells[r][c+1])   numBlock += 8  // 4    2 5
            if (cells[r+1] && cells[r+1][c+1]) numBlock += 16 // 5    3 6
            if (cells[r+2] && cells[r+2][c+1]) numBlock += 32 // 6    7 8
            if (cells[r+3] && cells[r+3][c])   numBlock += 64 // 7
            if (cells[r+3] && cells[r+3][c+1]) numBlock += 128// 8
            cliQr += String.fromCodePoint(numBlock)
          }
          cliQr += space
        }        
        return cliQr
      }
    }
  }
</script>
