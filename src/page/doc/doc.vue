<template>
  <div>
    <div class="left">
      <div class="btnGroup">
        <button class="new" @click="newCreate()">新建</button>
        <button class="remove" @click="remove">删除</button>
        <button class="send" @click="sendMessage()">发送消息</button>
      </div>
      <el-table class="table"
        ref="singleTable"
        :data="tableData"
        border
        highlight-current-row
        @current-change="showDoc"
        height="250"
        style="width: 100%">
        <el-table-column
          property="title"
          label="标题">
        </el-table-column>
        <el-table-column
          property="pubTime"
          width="120px"
          label="发布日期">
        </el-table-column>
      </el-table>
      <div class="pagation">
        <el-pagination
          @current-change="currentPagefun"
          :current-page.sync="currentPage"
          :page-size="pageSize"
          layout="total, prev, pager, next"
          :total="total">
        </el-pagination>

      </div>

    </div>
    <div class="right">
      <router-view class="rightView"></router-view>
    </div>

    <div class="alert" v-if="alert">
      <div class="alertBox">
        <i class="iconfont icon-cuo" @click="closeLayer"></i>
        <h3>发送消息</h3>
        <div class="tableBox">
          <div class="flex1">
            <p>人员</p>
            <div class="flex1Header">
              <div class="inputRow">
                <div class="input">
                  <i class="iconfont icon-search_icon"></i>
                  <input type="text" v-model="searchV" placeholder="请输入搜索信息">
                </div>
                <button class="search" @click="search">查询</button>
              </div>
              <div class="radio">
                <el-radio v-model="radio" label="department">部门</el-radio>
                <el-radio v-model="radio" label="username">姓名</el-radio>
                <el-radio v-model="radio" label="phone">电话</el-radio>
              </div>
            </div>
            <div class="flex1Table">
              <el-table class="leftBox"
                        border
                        align="center"
                        height="100%"
                ref="multipleTable"
                :data="allPData"
                tooltip-effect="dark"
                style="width: 100%"
                @selection-change="handleSelectionChange">
                <el-table-column
                  type="selection">
                </el-table-column>
                <el-table-column
                  label="部门">
                  <template slot-scope="scope">{{ scope.row.department }}</template>
                </el-table-column>
                <el-table-column
                  prop="typeName"
                  label="岗位">
                </el-table-column>
                <el-table-column
                  prop="username"
                  label="姓名"
                  show-overflow-tooltip>
                </el-table-column>
                <el-table-column
                prop="phone"
                label="手机号"
                width="120px"
                show-overflow-tooltip>
              </el-table-column>
              </el-table>
            </div>

          </div>
          <div class="flex2">
            <div>
              <button @click="add">添加 <i class="iconfont icon-xiangyou"></i></button>
            </div>
            <div>
              <button @click="removeAdd"><i class="iconfont icon-arrow-left"></i> 移除</button>
            </div>
          </div>
          <div class="flex1">
            <p>添加人员</p>
            <div class="flex1Header">
              <div class="inputRow">
                <button class="add" @click="addPhone">添加号码</button>
                <div class="input">
                  <i class="iconfont icon-search_icon"></i>
                  <input type="text" v-model="phoneV" placeholder="输入手机号">
                </div>
                <button class="sendM" @click="sendM">发送消息</button>
              </div>
            </div>
            <div class="flex1Table">
              <el-table class="leftBox"
                        border
                        ref="multipleTable"
                        :data="selectPData"
                        tooltip-effect="dark"
                        height="100%"
                        style="width: 100%"
                        @selection-change="removeSelectionChange">
                <el-table-column
                  type="selection"
                  width="55">
                </el-table-column>
                <el-table-column
                  label="部门">
                  <template slot-scope="scope">{{ scope.row.department }}</template>
                </el-table-column>
                <el-table-column
                  prop="typeName"
                  label="岗位">
                </el-table-column>
                <el-table-column
                  prop="username"
                  label="姓名"
                  show-overflow-tooltip>
                </el-table-column>
                <el-table-column
                  prop="phone"
                  label="手机号"
                  width="120px"
                  show-overflow-tooltip>
                </el-table-column>
              </el-table>
            </div>
            <div class="flex1Message">
              <p>消息</p>
              <!--<span>{{textV.length}}/{{textVCount}}</span>-->
              <textarea name="" id="" v-model="textV">
              </textarea>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
    export default require('./docCtr');
</script>
<style scoped>
  .left {
    flex: 3;
    height:100%;
    margin-right:20px;
    box-sizing: border-box;
    padding:20px;
    background-color: white;
    display: flex;
    flex-direction: column;
  }
  .btnGroup {
    width:80%;
    margin:0 auto;
    display: flex;
    justify-content: space-between;
  }
  .btnGroup button {
    width:80px;
    height:35px;
    color:white;
    text-align: center;
    line-height:35px;
    border-radius: 35px;
    outline: none;
    border:none;
    cursor: pointer;
  }
  .new {
    background-color: #4768f3;
  }
  .remove{
    background-color: #f29150;
  }
  .send {
    background-color: #f2446d;
  }
  .table {
    border-radius: 10px 10px 0 0;
    margin-top:20px;
  }
  .pagation {
    width:100%;
    text-align: center;
    padding:30px 0;
    border:1px solid #ebeef5;
    border-radius:0 0 10px 10px;
    box-sizing: border-box;
  }
  .right {
    flex: 6;
    height:100%;
    box-sizing: border-box;
    padding:20px;
    background-color: white;
    /*position:relative;*/
  }
  .rightView {
    width:100%;
    height:100%;
  }
  .alert {
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    padding:40px;
    box-sizing: border-box;
    background-color: rgba(0,0,0,.4);
    z-index:10;
  }
  .alertBox {
    width:100%;
    height:100%;
    background-color: white;
    border-radius: 10px;
    position:relative;
  }
  .alertBox {
    text-align: center;
    color:#4768f3;
    display: flex;
    flex-direction: column;
    padding:30px;
    box-sizing: border-box;
  }
  .alertBox .icon-cuo {
    position:absolute;
    top:10px;
    right:10px;
    font-size:20px;
    font-weight: bold;
    z-index:11;
  }
  .alertBox h3 {
    margin-bottom:30px;
  }
  .alertBox .tableBox {
    flex: 1;
    display: flex;
    flex-direction: row;
  }
  .flex1 {
    flex:5;
    height:calc(100%);
    display: flex;
    flex-direction: column;
  }
  .flex1Header {
    width:100%;
    box-sizing: border-box;
    padding:20px 10px;
    border:1px solid #ebeef5;
  }
  .flex1Table {
    flex: 1;
    height:100px;
  }
  .leftBox {
    height:100%;
  }
  .inputRow {
    width:100%;
    display: flex;
    flex-direction: row;
  }
  .input {
    flex: 1;
    height:35px;
    border-radius: 35px;
    border:1px solid #c2c2c2;
    position:relative;
    box-sizing: border-box;
  }
  .input i {
    position:absolute;
    font-size:20px;
    top:6px;
    left:15px;
  }
  .input input {
    width:100%;
    height:35px;
    line-height:35px;
    background-color: transparent;
    outline: none;
    border:none;
    border-radius: 35px;
    font-size:14px;
    text-indent:40px;
    box-sizing: border-box;
    padding-right:20px;
  }
  .search {
    width:100px;
    height:35px;
    color:white;
    text-align: center;
    line-height:35px;
    border-radius: 35px;
    outline: none;
    border:none;
    cursor: pointer;
    background-color: #4768f3;
    margin-left:10px;
  }
  .add {
    width:100px;
    height:35px;
    color:white;
    text-align: center;
    line-height:35px;
    border-radius: 35px;
    outline: none;
    border:none;
    cursor: pointer;
    background-color: #4768f3;
    margin-right:10px;
  }
  .radio {
    margin-top:5px;
    padding-left:5px;
    text-align: left;
  }
  .flex1 p {
    margin-bottom:10px;
  }

  .flex2 {
    flex: 2;
    height:100%;
    padding-top:150px;
    box-sizing: border-box;
  }
  .flex2 button {
    width:100px;
    height:35px;
    color:white;
    text-align: center;
    line-height:35px;
    border-radius: 35px;
    outline: none;
    border:none;
    cursor: pointer;
    margin-bottom:20px;
  }
  .flex2 div:nth-of-type(1) button{
    background-color: #f29150;
  }
  .flex2 div:nth-of-type(2) button{
    background-color: #25ada4;
  }
  .sendM {
    width:100px;
    height:35px;
    color:white;
    text-align: center;
    line-height:35px;
    border-radius: 35px;
    outline: none;
    border:none;
    cursor: pointer;
    background-color: #f2446d;
    margin-left:10px;
  }

  .flex1Message {
    height:150px;
    margin-top:30px;
    position:relative;
  }
  .flex1Message textarea{
    width:100%;
    height:120px;
    resize: none;
    outline: none;
    border:1px solid #ebeef5;
    box-sizing: border-box;
    padding:10px;
  }
  .flex1Message span {
    position:absolute;
    font-size:14px;
    bottom:0px;
    right:16px;
  }
  .el-table {
    cursor: pointer;
  }
</style>
