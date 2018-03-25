<template>
  <div id="app">
    <header>
      <span>BNK48 Sort</span>
    </header>
    <div class="container">
      <div class="instruction">
        จัดอันดับเมมเบอร์ที่ชื่นชอบด้วยการเลือกทีละคู่ หรือถ้าตัดใจไม่ได้ก็ให้เลือกเสมอนะ เลือกไปเรื่อยๆ (ประมาณ 100 ครั้ง) เสร็จแล้วจะมีผลลัพธ์ออกมาให้ดู
      </div>
      <template v-if="ranking.length == 0">
        <main v-if="L && R">
          <p>Battle No. <span id="num-battle">{{ num_battle }}</span></p>
          <div class="mem mem-left" @click="choseleft">
            <div class="photo-wrap">
              <transition name="fade">
                <img class="photo" :key="L.slug" :src="'./static/img/profiles/' + L.slug + '-03.jpg'">
              </transition>
            </div>
            <div class="name">
              <p class="nickname">{{ L.nick }}</p>
              <p class="realname">{{ L.name }}</p>
            </div>
          </div>
          <div class="mem mem-right" @click="choseright">
            <div class="photo-wrap">
              <transition name="fade">
                <img class="photo" :key="R.slug" :src="'./static/img/profiles/' + R.slug + '-03.jpg'">
              </transition>
            </div>
            <div class="name">
              <p class="nickname">{{ R.nick }}</p>
              <p class="realname">{{ R.name }}</p>
            </div>
          </div>
          <div class="clear"></div>
          <p><button @click="chosedraw" type="button" class="button" id="draw-button">ไม่รู้จะเลือกใคร</button></p>
          <hr>
        </main>
      </template>
      <section class="result" v-show="ranking.length > 0">
        <table>
          <thead>
            <tr>
              <th>#</th>
              <th>ชื่อ</th>
              <th>คะแนน</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(r, i) in ranking" :key="i">
              <td>{{ r.rank }}</td>
              <td class="name"><span class="nickname">{{ r.member.nick }}</span> {{ r.member.name }}</td>
              <td>{{ r.score }} pts</td>
            </tr>
          </tbody>
        </table>
        <p class="restart-wrap">
          <button @click="restart" type="button" class="button" id="restart-button">เริ่มเล่นใหม่</button>
        </p>
      </section>
      <section class="share">
        <p class="text-center">ชวนเพื่อนมาเล่นสิ!</p>
        <p class="text-center">
          <a class="twitter-share-button" href="https://twitter.com/intent/tweet?text=#BNK48%20Sort%20-%20%E0%B8%88%E0%B8%B1%E0%B8%94%E0%B8%AD%E0%B8%B1%E0%B8%99%E0%B8%94%E0%B8%B1%E0%B8%9A%E0%B9%80%E0%B8%A1%E0%B8%A1%E0%B9%80%E0%B8%9A%E0%B8%AD%E0%B8%A3%E0%B9%8C%E0%B9%83%E0%B8%99%E0%B8%94%E0%B8%A7%E0%B8%87%E0%B9%83%E0%B8%88" data-size="large">Tweet</a>
          <iframe src="https://www.facebook.com/plugins/share_button.php?href=https%3A%2F%2F48.meta.in.th%2F&layout=button_count&size=large&mobile_iframe=true&appId=136300256387360&width=84&height=28" width="84" height="28" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true"></iframe>
        </p>
      </section>        
      <section class="history">
        <div class="row column">
          <dl>
            <dt>2018-03-25</dt>
            <dd>เปลี่ยนรูปติดบัตร</dd>
          </dl>
          <dl>
            <dt>2018-02-07</dt>
            <dd>เปลี่ยนรูปติดบัตร / เอาเมมเบอร์ที่จบการศึกษาแล้วออก</dd>
          </dl>
          <dl>
            <dt>2017-06-10</dt>
            <dd>ปรับวิธีคิดคะแนน / ปรับหน้าตา / เปลี่ยนรูปติดบัตร</dd>
          </dl>
          <dl>
            <dt>2017-02-17</dt>
            <dd>เวอร์ชันทดสอบ</dd>
          </dl>
        </div>
      </section>
      <section class="contact">
        <div class="row column">
          <p>แจ้งปัญหา, บั๊ก ได้ที่ Twitter <a href="https://twitter.com/pittaya" title="Twitter @pittaya">@pittaya</a> 🙏</p>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
const _ = require('lodash')
export default {
  name: 'app',
  data () {
    return {
      members: [
        {'name': 'เฌอปราง อารีย์กุล', 'nick': 'Cherprang', 'slug': 'cherprang'},
        {'name': 'อิสราภา ธวัชภักดี', 'nick': 'Tarwaan', 'slug': 'tarwaan'},
        {'name': 'เจนนิษฐ์ โอ่ประเสริฐ', 'nick': 'Jennis', 'slug': 'jennis'},
        {'name': 'จิรดาภา อินทจักร', 'nick': 'Pupe', 'slug': 'pupe'},
        {'name': 'กานต์ธีรา วัชรทัศนกุล', 'nick': 'Noey', 'slug': 'noey'},
        {'name': 'กรภัทร์ นิลประภา', 'nick': 'Kate', 'slug': 'kate'},
        {'name': 'กุลจิราณัฐ อินทรศิลป์', 'nick': 'Jane', 'slug': 'jane'},
        {'name': 'มนัญญา เกาะจู', 'nick': 'Nink', 'slug': 'nink'},
        {'name': 'เมษา จีนะวิจารณะ', 'nick': 'Maysa', 'slug': 'maysa'},
        {'name': 'มิลิน ดอกเทียน', 'nick': 'Nam-Neung', 'slug': 'namneung'},
        {'name': 'มิโอริ โอคุโบะ', 'nick': 'Miori', 'slug': 'miori'},
        {'name': 'ณปภัช วรพฤทธานนท์', 'nick': 'Jaa', 'slug': 'jaa'},
        {'name': 'ณัฐรุจา ชุติวรรณโสภณ', 'nick': 'Kaew', 'slug': 'kaew'},
        {'name': 'นายิกา ศรีเนียน', 'nick': 'Can', 'slug': 'can'},
        {'name': 'ปณิศา ศรีละเลิง', 'nick': 'Mild', 'slug': 'mind'},
        {'name': 'พัศชนันท์ เจียจิรโชติ', 'nick': 'Orn', 'slug': 'orn'},
        {'name': 'พิชญาภา นาถา', 'nick': 'Namsai', 'slug': 'namsai'},
        {'name': 'พิมรภัส ผดุงวัฒนะโชค', 'nick': 'Mobile', 'slug': 'mobile'},
        {'name': 'แพรวา สุธรรมพงษ์', 'nick': 'Music', 'slug': 'music'},
        {'name': 'ปัญสิกรณ์ ติยะกร', 'nick': 'Pun', 'slug': 'pun'},
        {'name': 'รินรดา อินทร์ไธสง', 'nick': 'Piam', 'slug': 'piam'},
        {'name': 'สวิชญา ขจรรุ่งศิลป์', 'nick': 'Satchan', 'slug': 'satchan'},
        {'name': 'สุชญา แสนโคต', 'nick': 'Jib', 'slug': 'jib'},
        {'name': 'วฑูศิริ ภูวปัญญาสิริ', 'nick': 'Korn', 'slug': 'korn'},
        {'name': 'วรัทยา ดีสมเลิศ', 'nick': 'Kaimook', 'slug': 'kaimook'},
        {'name': 'รินะ อิสึตะ', 'nick': 'Izurina', 'slug': 'izutarina'}
      ],
      L: null,
      R: null,
      battles: [],
      results: [],
      current_battle: 0,
      current_result: 0,
      num_battle: 1,
      current_size: 1,
      l_cur: 0,
      r_cur: 0,
      finished: false,
      scores: {},
      ranking: []
    }
  },
  methods: {
    nextbattle () {
      if (this.battles[this.current_battle][0].length === this.l_cur) {
        for (let i = this.r_cur; i < this.battles[this.current_battle][1].length; i++) {
          this.results[this.current_result].push(this.battles[this.current_battle][1][i])
        }
        this.l_cur = 0
        this.r_cur = 0
        this.current_battle += 1
        this.current_result += 1
      } else if (this.battles[this.current_battle][1].length === this.r_cur) {
        for (let i = this.l_cur; i < this.battles[this.current_battle][0].length; i++) {
          this.results[this.current_result].push(this.battles[this.current_battle][0][i])
        }
        this.l_cur = 0
        this.r_cur = 0
        this.current_battle += 1
        this.current_result += 1
      }

      if (typeof this.battles[this.current_battle] !== 'undefined' && this.battles[this.current_battle].length === 1) {
        this.results[this.current_result].push(this.battles[this.current_battle][0][0])
        this.l_cur = 0
        this.r_cur = 0
        this.current_battle += 1
        this.current_result += 1
      }

      if (this.current_battle === this.battles.length) {
        this.current_battle = 0
        this.battles = _.chunk(this.results, 2)
        this.current_result = 0
        this.results = Array.from({ length: this.battles.length }, () => [])
      }
      if (this.battles[0].length === 1) {
        this.finished = true
        let beforeRank = _.map(this.scores, (v, k) => {
          return { score: v, member: _.find(this.members, m => m.slug === k) }
        })
        let sorted = _.reverse(_.sortBy(beforeRank, 'score'))
        let currentRank = 1
        sorted[0].rank = 1
        for (let i = 1; i < sorted.length; i++) {
          if (sorted[i].score === sorted[i - 1].score) {
            sorted[i].rank = sorted[i - 1].rank
            currentRank += 1
          } else {
            currentRank += 1
            sorted[i].rank = currentRank
          }
        }
        this.ranking = sorted
        return
      }

      this.L = this.battles[this.current_battle][0][this.l_cur]
      this.R = this.battles[this.current_battle][1][this.r_cur]
      this.num_battle += 1
    },
    choseleft () {
      this.results[this.current_result].push(this.R)
      this.scores[this.L.slug] = Math.max(this.scores[this.L.slug], this.scores[this.R.slug]) + 1
      for (let i = this.l_cur + 1; i < this.battles[this.current_battle][0].length; i++) {
        this.scores[this.battles[this.current_battle][0][i].slug] += 1
      }
      this.r_cur += 1
      this.nextbattle()
    },
    choseright () {
      this.results[this.current_result].push(this.L)
      this.scores[this.R.slug] = Math.max(this.scores[this.L.slug], this.scores[this.R.slug]) + 1
      for (let i = this.r_cur + 1; i < this.battles[this.current_battle][1].length; i++) {
        this.scores[this.battles[this.current_battle][1][i].slug] += 1
      }
      this.l_cur += 1
      this.nextbattle()
    },
    chosedraw () {
      this.results[this.current_result].push(this.L)
      this.scores[this.R.slug] = Math.max(this.scores[this.L.slug], this.scores[this.R.slug])
      this.l_cur += 1
      this.nextbattle()
    },
    restart () {
      document.location.reload()
    }
  },
  beforeMount () {
    this.members = _.shuffle(this.members)
    this.battles = _.chunk(_.chunk(this.members, 1), 2)
    this.results = Array.from({ length: this.battles.length }, () => [])
    this.L = this.battles[this.current_battle][0][this.l_cur]
    this.R = this.battles[this.current_battle][1][this.r_cur]
    for (let i = 0; i < this.members.length; i++) {
      this.scores[this.members[i].slug] = 0
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
}
a {
  color: #c599bc;
}

#app {
  font-family: -apple-system,BlinkMacSystemFont,"Helvetica Neue",Roboto,Thonburi,Arial,Tahoma,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.container {
  max-width: 960px;
  margin: 0px auto;
}
main {
  text-align: center;
  margin: 10px;
}
main .mem {
  width: 49%;
  float: left;
  border: 1px solid #dedede;
  box-shadow: 0px 2px 4px #ededed;
  cursor: pointer;
}
main .mem:hover {
  box-shadow: 0px 2px 8px #cdcdcd;
}
main .mem-left {
  margin-right: 2%;
}
main .mem .photo-wrap {
  width: 100%;
  height: 0px;
  padding-bottom: 100%;
  overflow: hidden;
  background-color: #ededed;
}
main .mem img.photo {
  width: 100%;
  height: auto;
}
.mem .name p {
  margin: 0.5rem;
}
.mem .nickname {
  font-weight: 600;
}
.mem .realname {
  font-size: 0.9em;
  color: #999;
}

header {
  margin: 0;
  height: 56px;
  padding: 0 16px 0 24px;
  background-color: #d5a9cc;
  color: #ffffff;
}

header span {
  display: block;
  position: relative;
  font-size: 20px;
  line-height: 1;
  letter-spacing: .02em;
  font-weight: 400;
  box-sizing: border-box;
  padding-top: 16px;
}
.instruction {
  padding: 1rem;
  font-size: 0.9rem;
  text-align: center;
}
section.result {
  margin: 1rem;
}
section.result table {
  width: 100%;
  margin: 0px;
  padding: 0px;
  border-spacing: 0px;
}
section.result table th {
  font-weight: 600;
  background-color: #ccc;
  padding: 0.5rem;
}
section.result table td {
  text-align: center;
  padding: 0.5rem;
  color: #333;
}
section.result table td.name {
  text-align: left;
  color: #999;
}
section.result table span.nickname {
  font-weight: 500;
  color: #333;
}
section.result table tbody tr:nth-child(odd) {
   background-color: #fafafa;
}
section.share {
  margin: 1rem 1rem 4rem 1rem;
  text-align: center;
}
section.history, section.contact {
  margin: 1rem;
  font-size: 0.9em;
}
section.history {
  color: #aaa;
}
section.history dt {
  font-weight: 600;
}
section.history dd {
  margin: 0px;
}
p.restart-wrap {
  text-align: center;
  margin-bottom: 4rem;
}
div.clear {
  clear: both;
}
button {
  -webkit-appearance: button;
  color: #fff;
  background-color: #d5a9cc;
  border-color: #c599bc;
  display: inline-block;
  padding: 6px 12px;
  margin-bottom: 0;
  font-size: 14px;
  font-weight: 400;
  line-height: 1.42857143;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-image: none;
  border: 1px solid transparent;
  border-radius: 4px;
}
button:hover {
  background-color: #c599bc;
}
.fade-enter {
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}
.fade-enter-active {
  transition: all .5s ease;
}
</style>
