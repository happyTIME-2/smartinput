<template>
    <div
    class="editor"
    @click="inputFocus"
  >
    <el-tag
      v-for="(item,index) in result"
      :key="index"
      class="result-tag"
      size="small"
      type="info"
      closable
      effect="light"
      @close="handleClose(index)"
    >
      <span v-if="typeof (item) === 'object'">{{ item[keyName] }}</span>
      <span v-else>{{ item }}</span>
    </el-tag>
    <label>
      <input
        type="text"
        v-model.trim="inputData"
        @keyup="keyEvent($event)"
        @focus="focusEvent"
        @blur="blurEvent"
        ref="input"
      >
    </label>
    <div
      class="tips-wrapper"
      v-if="showTips"
      @mousedown="mousedownEvent"
    >
      <i
        class="el-icon-loading"
        v-if="searchLoading"
      />
      <ul>
        <li
          v-for="(item, index) in tipsList"
          :key="index"
          @click="clickSelect(index)"
          :class="index === focusIndex ? 'active' : ''"
        >
          <template v-if="typeof (item) === 'object'">
            <span class="avatar">
              <el-image
                :src="'\/\/dcloud.oa.com/Public/Avatar/'+item[keyName]+'.png'"
                lazy
              />
            </span>
            {{ item[keyName] }}
          </template>
          <template v-else>
            <span class="avatar">
              <el-image
                :src="'\/\/dcloud.oa.com/Public/Avatar/'+item+'.png'"
                lazy
              />
            </span>
            {{ item }}
          </template>
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
  import _ from 'lodash';
  import axios from 'axios';

  export default {
    name: 'TgSmartInput',
    props: {
      multiple: {
        type: Boolean,
        default: true,
      },
      dataApi: {
        type: String,
        default: 'String',
      },
      memberList: {
        type: Array,
        required: false
      },
      searchKey: {
        type: String,
        required: false,
      }
    },
    data() {
      return {
        result: [], // 多个最终的输入结果
        tipsList: [], // 提示结果，通过后端api搜索得到，最多10条
        inputData: '', // 输入框当前输入值，用于api搜索的参数值
        showTips: false, // 当搜索返回结果的时候，tipsList长度不为0时，显示tipsList
        searchLoading: false,
        shouldHiddenTips: false, // 控制input控件blur的时候是否可以隐藏tipsList
        focusIndex: 0, // 当前选中index
        keyName: 'name', // 如果搜索的结果数据组项为object，可以通过传入的keyName值来确定tipsList对应对象的那个key-value值
        canDelete: false, // 控制退格事件是否执行delete事件，进行选中对象删除
      };
    },
    methods: {
      handleClose(index) {
        this.result.splice(index, 1);
      },
      searchMember() {
        if (this.showTips.length === 0) {
          this.showTips = true;
          this.searchLoading = true;
        }
        if (this.memberList === undefined && this.dataApi === 'String') {
          console.log('memberList跟dataApi不能同时为空，否则无法进行数据搜索！');
          return ;
        }
        if (this.memberList !== undefined) {
          const pattern = new RegExp(`^${this.inputData}`);
          const pattern1 = new RegExp(this.inputData);
          this.tipsList = this.memberList.filter((item) => {
            if (typeof (item) === 'object' && Object.prototype.hasOwnProperty.call(item, 'name')) {
              if (this.searchKey !== undefined) {
                this.keyName = this.searchKey;
              }
              if (pattern.test(item[this.keyName])) return item;
            } else if (pattern.test(item)) {
              return item;
            }
            return false;
          }).slice(0,10);

          if (this.tipsList.length === 0) {
            this.tipsList = this.memberList.filter((item) => {
              if (typeof (item) === 'object' && Object.prototype.hasOwnProperty.call(item, 'name')) {
                if (this.searchKey !== undefined) {
                  this.keyName = this.searchKey;
                }
                if (pattern1.test(item[this.keyName])) return item;
              } else if (pattern1.test(item)) {
                return item;
              }
              return false;
            }).slice(0,10);
          }

          this.searchLoading = false;
          if (this.result.length > 0) this.focusIndex = 0;
          if (this.tipsList.length === 0) this.tipsList.push({ [this.keyName]: 'no result！' });
        } else {
          axios.get(this.dataApi + this.inputData).then((response) => {
            this.searchLoading = false;
            if (this.searchKey !== undefined) {
              this.keyName = this.searchKey;
            }
            this.tipsList = response.data.data.members;
            this.showTips = true;
            if (this.result.length > 0) this.focusIndex = 0;
            if (this.tipsList.length === 0) this.tipsList.push({ [this.keyName]: 'no result！' });
            // eslint-disable-next-line no-console
          }).catch((e) => { console.log(e); });
        }
      },
      selectMember(index) {
        this.result.push(this.tipsList[index]);
      },
      keyEvent(e) {
        switch (e.keyCode) {
        case 38:
          this.focusIndex = (this.focusIndex - 1 + this.tipsList.length) % this.tipsList.length;
          break;
        case 40:
          this.focusIndex = (this.focusIndex + 1 + this.tipsList.length) % this.tipsList.length;
          break;
        case 13:
          if (!this.searchLoading && this.focusIndex < this.tipsList.length) this.selectOne();
          break;
        case 8:
          if (this.inputData === '') {
            this.$nextTick(() => {
              this.canDelete = true;
            });
          }
          if (this.result.length > 0 && this.canDelete) this.deleteOne();
          break;
        default:
          break;
        }
      },
      selectOne() {
        if (typeof (this.tipsList[0]) === 'object') {
          // eslint-disable-next-line no-var,vars-on-top
          var noResult = this.tipsList[0].name.indexOf('no result！') === -1;
        } else {
          // eslint-disable-next-line block-scoped-var
          noResult = this.tipsList[0].indexOf('no result！') === -1;
        }

        // eslint-disable-next-line block-scoped-var,max-len
        if (this.multiple && this.result.indexOf(this.tipsList[this.focusIndex]) === -1 && noResult) {
          this.result.push(this.tipsList[this.focusIndex]);
        }

        // eslint-disable-next-line block-scoped-var,max-len
        if (!this.multiple && this.result.indexOf(this.tipsList[this.focusIndex]) === -1 && noResult) {
          this.result.splice(0, 1, this.tipsList[this.focusIndex]);
        }
      },
      deleteOne() {
        if (this.inputData === '') this.result.splice(this.result.length - 1);
      },
      clickSelect(index) {
        this.focusIndex = index;
        this.$refs.input.focus();
        this.selectOne();
      },
      inputFocus() {
        this.$refs.input.focus();
      },
      blurEvent() {
        if (this.shouldHiddenTips) this.showTips = false;
      },
      focusEvent() {
        if (this.tipsList.length !== 0) this.showTips = true;
        this.shouldHiddenTips = true;
      },
      mousedownEvent() {
        this.shouldHiddenTips = false;
        this.showTips = true;
        this.$nextTick(() => {
          this.$refs.input.focus();
        });
      },
    },
    watch: {
      inputData(newVal) {
        if (newVal === '') {
          this.tipsList = [];
          this.showTips = false;
          return;
        }
        this.canDelete = false;
        if (this.tipsList.length === 0) {
          this.showTips = true;
          this.searchLoading = true;
        }
        this.debouncedSearchMember();
      },
    },
    created() {
      this.debouncedSearchMember = _.debounce(this.searchMember, 500);
    },
  };
</script>
<style scoped>
  .editor{padding: 4px 10px;border-radius: 4px;background-color: #fff;
    box-sizing: border-box;border: 1px solid #dcdfe6;position: relative;
    transition: border-color .2s cubic-bezier(.645,.045,.355,1);min-height: 34px;}
  .editor .result-tag{margin-right: 6px;}
  .editor .tips-wrapper{position: absolute;z-index: 1001;border: 1px solid #e4e7ed;
    border-radius: 4px;background-color: #fff;box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
    box-sizing: border-box;margin: 5px 0;min-height: 60px;min-width: 160px;
    display: flex;}
  .editor .tips-wrapper .el-icon-loading{align-self: center;margin: 0 auto;display: block;}
  .editor .tips-wrapper ul{list-style: none;padding: 6px 0;margin: 0;
    box-sizing: border-box;width:100%}
  .editor li{font-size: 16px;padding: 2px 20px;position: relative;white-space: nowrap;
    overflow: hidden;text-overflow: ellipsis;color: #606266;height: 42px;
    line-height: 42px;cursor: pointer;display: block;}
  .editor li:hover{background-color: #f5f7fa;}
  .editor li.active{color: #409eff;background-color: #fff;}
  .editor label{height: 26px;line-height:26px;margin: 0}
  .editor input{border: none;height: 100%;width: 100px}
  .editor input,.el-icon-loading {display:inline-block;}
  .editor input:focus{outline: none;border: none}
  .editor .avatar{display:inline-block;width: 32px;height: 32px;
    border-radius: 50%;overflow: hidden;vertical-align:middle;margin-right:6px;}
</style>