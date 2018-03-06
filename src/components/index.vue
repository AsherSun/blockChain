<template>
  <div class="blockChain">
    <h1 class="blockChain-title">区块链</h1>
    <div class="blockChain-section">
      <ul class="blockChain-section-list">
        <li
          class="blockChain-section-list-item"
          v-for="(item, index) in blockData">
          <div
            v-if="index !== 0"
            class="icon up-down-motion">
            <i
              class="iconfont wpt-block-chain-16"></i>
          </div>
          <div class="desc before-hash">
            <div class="desc-left ">BEFORE HASH</div>
            <div class="desc-right">{{item.beforeHash}}</div>
          </div>
          <div class="desc id">
            <div class="desc-left">ID</div>
            <div class="desc-right">{{item.id}}</div>
          </div>
          <div class="desc desc-content">
            <div class="desc-left">DATA</div>
            <div class="desc-right">
              <input
                type="text"
                @keyup="keyupFn(index)"
                v-model.lazy="item.content">
              <i class="iconfont wpt-block-chain-bookmark"></i>
            </div>
          </div>
          <div class="desc now-hash">
            <div class="desc-left">HASH</div>
            <div class="desc-right">{{item.hash}}</div>
          </div>
          <div class="cenesis">
            <p v-if="index === 0">GENESIS BLOCK</p>
            <p v-else>BLOCK # {{index}}</p>
            <span class="cenesis-time">{{item.time}}</span>
          </div>
        </li>
      </ul>
      <div class="entry">
        <div class="entry-header">
          <div class="entry-header-left">DATA</div>
          <div class="entry-header-right">
            <input type="text" v-model="value">
            <i class="iconfont wpt-block-chain-bookmark"></i>
          </div>
        </div>
        <button class="entry-add"  @click="addNewBlock">ADD NEW BLOCK</button>
      </div>
    </div>
  </div>
</template>

<script>
  import {SHA256} from '@/assets/js/sha256'

  export default {
    methods: {
      // 添加新的区块
      addNewBlock() {
        this.haseFn()
        let i = this.index
        this.index++
        this.blockData.push({
          time: new Date(),
          beforeHash: this.blockData[i].hash,
          id: this.index,
          content: this.value,
          hash: this.nowHash
        })
        this.value = ''
      },
      // hash 计算
      haseFn() {
        let str = this.blockData[this.index].hash + this.value + new Date().getTime()
        this.nowHash = SHA256(str)
      },
      keyupFn(i) {
        let len = this.blockData.length
        for(; i<len; i++){
         // console.log(this.blockData[i])
         this.preveHash(i)
        }
      },
      // hash 计算
      preveHash(i) {
        let index = i > 0 ? i - 1 : 0
        this.blockData[i].hash = SHA256(this.blockData[index].hash + this.blockData[i].value + new Date().getTime())
        this.blockData[i].beforeHash = this.blockData[index].hash
      }
    },
    created() {
    },
    data() {
      return {
        value: '',
        nowHash: '',
        index: 0,
        blockData: [{
          beforeHash: '0',
          id: 0,
          content: 'Welcome to Blockchain Demo 2.0!',
          hash: '000dc75a315c77a1f9c98fb6247d03dd18ac52632d7dc6a9920261d8109b37cf',
          time: new Date()
        }]
      }
    }
  }
</script>

<style lang="less">
  .blockChain {
    max-width: 1200px;
    min-width: 640px;
    margin: 0 auto;
    &-title {
      text-align: center;
      line-height: 70px;
      font-size: 18px;
      font-weight: bolder;
    }
    .blockChain-section-list-item {
      width: 600px;
      position: relative;
      margin: 0 auto;
      background-color: #FFF;
      padding: 15px;
      margin-bottom: 10px;
      .desc {
        display: flex;
        flex-flow: row nowrap;
        font-size: 14px;
        align-items: center;
        line-height: 1.7;
        color: rgba(0, 0, 0, 0.65);
        .desc-left {
          width: 90px;
          flex-shrink: 0;
        }
        &.before-hash {
          margin-bottom: 15px;
          .desc-right {
            color: #52c41a;
          }
        }
        &.id {
          margin-bottom: 10px;
        }
        &.desc-content {
          margin-bottom: 15px;
          .desc-left {
            height: 25px;
            line-height: 25px;
            border: 1px solid #d9d9d9;
            text-align: center;
            border-right: none;
            color: #333;
          }
          .desc-right {
            position: relative;
            margin-left: 0;
            flex: 1;
            i {
              position: absolute;
              top: 0;
              left: 5px;
            }
          }
          input {
            height: 25px;
            width: 100%;
            border: 1px solid #d9d9d9;
            outline: none;
            border-top-right-radius: 5px;
            border-bottom-right-radius: 5px;
            text-indent: 25px;
            color: #b1afaf;
            &:hover {
              border: 1px solid #40a9ff;
              box-shadow: 0 0 5px #40a9ff;
            }
          }
        }
        &.now-hash {
          font-size: 12px;
          margin-bottom: 10px;
          .desc-right {
            color: #52c41a;
            padding: 3px;
            border-radius: 5px;
            border: 1px solid rgba(82, 196, 26, 0.4);
            line-height: 1;
          }
        }
      }
      .cenesis {
        display: flex;
        flex-flow: row nowrap;
        padding-top: 20px;
        padding-bottom: 20px;
        p {
          font-size: 20px;
          margin-right: 15px;
          color: rgba(0, 0, 0, 0.6);
        }
        &-time {
          font-size: 12px;
          align-self: flex-end;
          color: #999;
        }
      }
      &:after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        border-bottom: 1px solid #d9d9d9;

      }
      &:hover {
        box-shadow: 1px 10px 15px rgba(183, 183, 183, 0.6);
      }
      .icon {
        display: block;
        height: 70px;
        line-height: 70px;
        text-align: center;
        margin: 20px 0;
        i {
          font-size: 24px;
        }
      }
    }
    .entry {
      width: 275px;
      margin: 80px auto;
      padding: 50px;
      display: flex;
      flex-flow: column nowrap;
      align-items: center;
      border-radius: 10px;
      box-shadow: 4px 5px 4px rgba(183, 183, 183, 0.6);
      &-header {
        display: flex;
        flex-flow: row nowrap;
        line-height: 25px;
        &-left {
          width: 90px;
          flex-shrink: 0;
          height: 25px;
          line-height: 25px;
          border: 1px solid #d9d9d9;
          text-align: center;
          border-right: none;
          color: #333;
          font-size: 14px;
        }
        &-right {
          position: relative;
          -webkit-box-flex: 1;
          -ms-flex: 1;
          flex: 1;
          input {
            height: 25px;
            width: 100%;
            border: 1px solid #d9d9d9;
            outline: none;
            border-top-right-radius: 5px;
            border-bottom-right-radius: 5px;
            text-indent: 25px;
            color: #b1afaf;
          }
          i {
            position: absolute;
            top: 0;
            left: 5px;
          }
        }
      }
      button {
        outline: none;
        margin-top: 30px;
        margin-bottom: 10px;
        border-radius: 6.25em;
        padding: 20px 25px;
        height: auto;
        background: -webkit-linear-gradient(45deg, #d4145a, #fbb03b);
        background: -o-linear-gradient(45deg, #d4145a, #fbb03b);
        background: linear-gradient(45deg, #d4145a, #fbb03b);
        border: none;
        color: #fff;
        -webkit-appearance: button;
      }
      &:hover {
        box-shadow: 1px 35px 35px rgba(183, 183, 183, 0.6);
      }
    }
  }

  .up-down-motion {
    animation-name: upDownMotion;
    animation-duration: 1s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-direction: alternate;
  }

  @keyframes upDownMotion {
    0% {
      transform: translate3d(0, -10px, 0);
    }
    50% {
      transform: translate3d(0, 20px, 0);
    }
    100% {
      transform: translate3d(0, 30px, 0);
    }
  }

</style>
