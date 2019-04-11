<template>
  <div class="letterLines">
    <img
      src="../assets/police.png"
      alt="police"
      class="police"
      :style="{transform: 'translate(' + police.x*40 + 'px,' + police.y*77 + 'px)'}"
    >
    <img
      src="../assets/thief.png"
      alt="thief"
      class="thief"
      :style="{transform: 'translate(' + thief.x*40 + 'px,' + thief.y*77 +'px)'}"
    >
    <div v-for="line in letterLines" :key="line.index">
      <span
        v-for="letter in line"
        :key="letter.in"
        :style="letter.isType ? 'color:red' : null"
        class="letter"
      >{{letter.value}}</span>
    </div>
    <el-dialog
      title="game over"
      :visible.sync="dialogVisible"
      width="30%"
      :show-close="false"
      center
    >
      <span>{{dialogText}}</span>
      <span slot="footer" class="dialog-footer">
        <el-button  type="primary" @click="backToMain">back to main</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "CLine",
  props: {
    row: Number,
    col: Number
  },
  data() {
    return {
      letterLines: [],
      currentLetter: "",
      rowIndex: 0,
      colIndex: 0,
      thief: { x: 0, y: 0 },
      police: { x: -3, y: 0 },
      dialogText: "",
      dialogVisible: false,
      difficulty: ""
    };
  },
  mounted: function() {
    let letterLines = [];
    const LETTERARR = [
      "a",
      "b",
      "c",
      "d",
      "e",
      "f",
      "g",
      "h",
      "i",
      "j",
      "k",
      "l",
      "m",
      "n",
      "o",
      "p",
      "q",
      "r",
      "s",
      "t",
      "u",
      "v",
      "w",
      "x",
      "y",
      "z"
    ];
    for (let i = 0; i < this.row; i++) {
      let line = [];
      for (let k = 0; k < this.col; k++) {
        let id = `${i}${k}`;
        let index = parseInt(Math.random() * 25);
        let value = LETTERARR[index];
        let letter = { value, isType: false, id };
        line.push(letter);
      }
      letterLines.push(line);
    }
    this.letterLines = letterLines;
    this.currentLetter = letterLines[0][0].value;
  },
  methods: {
    init: function(difficulty) {
      this.difficulty = difficulty;
      let time = 5000,
        policeX = -3;
      switch (difficulty) {
        case "middle":
          time = 3000;
          policeX = -2;
          break;
        case "difficult":
          time = 1000;
          policeX = -1;
          break;
      }
      this.police.x = policeX;
      this.interval = setInterval(() => {
        this.police.x++;
        if (this.police.y >= this.thief.y && this.police.x >= this.thief.x) {
          clearInterval(this.interval);
          this.dialogText = "you lose";
          this.dialogVisible = true;
        } else {
          const length = this.letterLines[0].length;
          if (this.police.x >= length) {
            this.police.y++;
            this.police.x = 0;
          }
        }
      }, time);
      window.addEventListener("keydown", e => {
        const typeLetter = e.key;
        // 行数未超过数据最大值
        if (this.rowIndex < this.letterLines.length) {
          // 列数未超过数据最大值
          if (this.colIndex < this.letterLines[this.rowIndex].length) {
            // 打出的字母与当前字母相同
            if (typeLetter === this.currentLetter) {
              // 更新字母颜色
              this.letterLines[this.rowIndex][this.colIndex].isType = true;
              // 列数index增加
              this.colIndex++;
              // 如果增加后的列数未超过最大值，更新当前字母,小偷横向移动
              if (this.colIndex < this.letterLines[this.rowIndex].length) {
                this.thief.x++;
                this.currentLetter = this.letterLines[this.rowIndex][
                  this.colIndex
                ].value;
                // 如果超过最大值，判断列数有没超过最大值
              } else {
                // 未超过，行数index增加，小偷移动到下一行
                if (this.rowIndex < this.letterLines.length) {
                  this.thief.y++;
                  this.thief.x = 0;
                  this.rowIndex++;
                  this.colIndex = 0;
                  if (this.rowIndex < this.letterLines.length) {
                    this.currentLetter = this.letterLines[this.rowIndex][
                      this.colIndex
                    ].value;
                  } else {
                    // 超过了，清除警察定时器，小偷胜利
                    clearInterval(this.interval);
                    // alert("you win");
                    this.dialogText = "you win";
                    this.dialogVisible = true;
                  }
                }
              }
            }
          }
        }
      });
    },
    backToMain: function() {
      this.$emit("backToMain");
      this.dialogVisible = false;
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.letterLines {
  position: relative;
  display: inline-block;
}
.thief,
.police {
  display: inline-block;
  position: absolute;
  left: 0;
  top: 0;
}
.thief {
  width: 37px;
  height: 50px;
  transition: transform 0.1s linear;
}
.police {
  width: 39px;
  height: 50px;
  z-index: 10;
  transition: transform 0.5s linear;
}
.letter {
  display: inline-block;
  width: 40px;
  height: 20px;
  text-align: center;
  overflow: hidden;
  vertical-align: middle;
  margin-top: 50px;
  margin-bottom: 5px;
  border-top: 1px solid #000;
  border-bottom: 1px solid #000;
}
</style>