
<script>
import HelloWorld from './components/HelloWorld.vue'
import LineEcharts from './components/LineEcharts/LineEcharts.vue'
import TableData from '@/components/TableData/TableData.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    LineEcharts,
    TableData
  },
  data: function () {
    return {
      componentsList: [
        {
          tag: 'LineEcharts',
          ion: 'fa fa-bar-chart',
          imgSrc: require('./assets/images/echarts.jpg')
        },
        {
          tag: 'HelloWorld',
          ion: 'fa fa-map',
          imgSrc: require('./assets/images/map.jpg')
        },
        {
          tag: 'TableData',
          ion: 'fa fa-table',
          imgSrc: require('./assets/images/table.jpg')
        }
      ],
      positionX: 0,
      positionY: 0,
      screenWidth: document.documentElement.clientWidth, // 屏幕宽度
      screenHeight: document.documentElement.clientHeight, // 屏幕高度
      translateLeft: 0,
      translateTop: 0,
      elementArray: []
    }
  },
  render: function (h) {
    var self = this
    return (
      <div id="app">
        <div class="edop-edit">
          <div class="edop-edit-header">
            <ul class="components-menu">
              {
                this.componentsList.map((item) => {
                  return (
                    <li class="components-group">
                      <i class={item.ion} />
                      <div class="coms-drop-list">
                        <table>
                          <tr>
                            <td>
                              {(() => {
                                return h('img', {
                                  attrs: {
                                    src: item.imgSrc
                                  },
                                  on: {
                                    click: () => {
                                      self.selectedComponent(item)
                                    } }
                                })
                              })()}
                            </td>
                          </tr>
                        </table>
                      </div>
                    </li>
                  )
                })
              }
            </ul>
          </div>

          <div class="edop-edit-main">
            <div class="left-edit-main" />
            <div class="center-edit-main">
              <div class="canvas-main">
                <div class="canvas-main-wrap" style={{ width: (this.screenWidth - 485) + 'px', height: (this.screenHeight - 40) + 'px' }}>
                  <div class="canvas-panel">
                    <div class="canvas-panel-main">
                      {this.elementArray.map(function (item, index) {
                        return (
                          <div class="main-component" onmousedown={self.move}>
                            <div class="tools">
                              <i class="fa fa-trash-o component-delete" onClick={self.deleteComponent.bind(this, index)}></i>
                            </div>
                            {(() => {
                              return h(item)
                            })()}
                          </div>
                        )
                      })}

                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="right-edit-main" />
          </div>
        </div>
      </div>
    )
  },
  methods: {
    move (e) {
      let odiv = e.target // 获取目标元素
      // console.log(odiv)
      console.log(e.target.tagName)
      if (e.target.tagName === 'DIV') { // 只有是div的时候才可移动
        // 算出鼠标相对元素的位置
        let disX = e.clientX - odiv.offsetLeft
        let disY = e.clientY - odiv.offsetTop
        document.onmousemove = (e) => { // 鼠标按下并移动的事件
          // 用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
          let left = e.clientX - disX
          let top = e.clientY - disY

          // 绑定元素位置到positionX和positionY上面
          this.positionX = top
          this.positionY = left
          console.log(left)
          console.log(top)
          // 移动当前元素
          odiv.style.left = left + 'px'
          odiv.style.top = top + 'px'
          // odiv.style.transform = 'translate(' + left+'px' + ','+ top + 'px'+ ')'
        }
        document.onmouseup = (e) => {
          document.onmousemove = null
          document.onmouseup = null
        }
      }
    },

    selectedComponent (item) {
      this.elementArray.push(item.tag)
      console.log(this.elementArray)
    },
    deleteComponent (index) {
      this.elementArray.splice(index, 1)
    }

  },
  mounted () {
    var _this = this
    window.onresize = function () { // 定义窗口大小变更通知事件
      _this.screenWidth = document.documentElement.clientWidth // 窗口宽度
      _this.screenHeight = document.documentElement.clientHeight // 窗口高度
    }
  }
}
</script>

<style lang="scss">
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  font-family: PingFangSC, "Microsoft YaHei", "Helvetica Neue", Arial,
    sans-serif !important;
  font-size: 12px;
  overflow: hidden;
  -webkit-font-smoothing: antialiased;
  min-width: 1024px;
}
#app {
  height: 100%;

  .edop-edit {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
    position: relative;
    .edop-edit-header {
      display: flex;
      height: 40px;
      z-index: 100;
      background: #222528;
      .components-menu {
        left: 200px;
        margin: 0;
        padding: 0;
        display: inline-flex;
        list-style: none;
        padding-left: 0;
        position: relative;
        margin-right: 10px;

        .components-group {
          cursor: pointer;
          height: 40px;
          width: 56px;
          color: #bcc9d4;
          margin-right: 10px;
          line-height: 35px;
          text-align: center;
          border-top: 2px solid transparent;

          &:hover {
            border-top-color: #00baff;
            color: #00baff;

            .coms-drop-list {
              display: block;

            }
          }

          > i {
            font-size: 20px;
          }

          .coms-drop-list {
            display: none;
            position: absolute;
            top: 40px;
            z-index: 10;
            background: 0 0;
            box-shadow: 4px 4px 8px 0 rgba(0, 0, 0, 0.5);

            .list-img {
                background: url(https://img.alicdn.com/tfs/TB184VLcPfguuRjSspkXXXchpXa-14-14.png);
              }
          }
        }
      }
    }
    .edop-edit-main {
      flex: 1;
      display: flex;
      flex-wrap: nowrap;
      position: relative;
      background: #1c1e22;
      .left-edit-main {
        flex: none;
        position: relative;
        width: 185px;
        height: 100%;
        background: #1d1f26;
        overflow: auto;
        display: flex;
        flex-direction: column;
      }
      .center-edit-main {
        flex: 1;
        background: #26282d;
        .canvas-main {
          position: relative;
          height: 100%;
          background: url(https://img.alicdn.com/tfs/TB184VLcPfguuRjSspkXXXchpXa-14-14.png)
            repeat;
          user-select: none;

        .canvas-main-wrap {
          overflow: auto;
          // position: relative;

          .canvas-panel {
                width: 1920px;
                height: 1080px;
                background-color: rgb(13, 42, 67);
                transform: scale(0.674074) translate(0px, 0px);
                position: absolute;
                transform-origin: 0 0;
                top: 80px;
                left: 60px;
                transition: .2s all ease-in-out;
                background-size: cover,contain;
                background-position: center,right bottom;
                background-repeat: no-repeat,no-repeat;
                box-shadow: rgba(0,0,0,.5) 0 0 30px 0;
                @media (min-width: 1890px) and (max-width: 1920px) {
				            transform: scale(0.674074) translate(0px, 0px);
                }
                 @media (min-width: 1800px) and (max-width: 1890px) {
				            transform: scale(0.624074) translate(0px, 0px);
                }
                  @media (min-width: 1750px) and (max-width: 1800px) {
				            transform: scale(0.574074) translate(0px, 0px);
                }
                   @media (min-width: 1650px) and (max-width: 1750px) {
				            transform: scale(0.474074) translate(0px, 0px);
                }
                   @media (min-width: 1550px) and (max-width: 1650px) {
				            transform: scale(0.434074) translate(0px, 0px);
                }
                    @media (min-width: 1450px) and (max-width: 1550px) {
				            transform: scale(0.404074) translate(0px, 0px);
                }
                    @media (min-width: 1366px) and (max-width: 1450px) {
				            transform: scale(0.374074) translate(0px, 0px);
                }
                    @media  (max-width: 1450px) {
				            transform: scale(0.374074) translate(0px, 0px);
                }

                .canvas-panel-main {
                  position: relative;
                }
          }

          .main-component {
            position: absolute;
            top: 0;
            left: 0;
            width: 580px;
            height: 300px;
            padding: 20px;
            background-color: #fff;
            transform: translate(0px, 0px);
            cursor: move;
            z-index: 999;
            opacity: 1;
            &:hover {
              .tools {
                display: block;
              }
            }
            .tools {
              position: absolute;
              right: -30px;
              top: 0px;
              background-color: #fff;
              padding: 10px;
              display: none;
              cursor: pointer;
              .component-delete {
              font-size: 24px;
              color: #000;

              &:hover {
                color: red;
              }

            }
            }

          }
          }
        }
      }
      .right-edit-main {
        width: 300px;
        height: 100%;
        z-index: 90;
        background: #1c1f25;
        position: relative;
        transition: width 0.25s ease-in-out;
      }
    }
  }
}
</style>
