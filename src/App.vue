<template>
  <div id="app" style="container">
    <div class="col-md-10 col-md-offset-1">
      <div class="page-header">
        <h3>基于Vue的组织架构树组件</h3>
      </div>
      <div class="row">
        <div class="col-md-8 col-md-offset-2">
          <form class="form-horizontal row">
            <div class="col-md-3">
              <div class="checkbox">
                <label>
                  <input type="checkbox" v-model="horizontal" > horizontal
                </label>
              </div>
            </div>
            <div class="col-md-3">
              <div class="checkbox">
                <label>
                  <input type="checkbox" v-model="collapsable"> collapsable
                </label>
              </div>
            </div>
            <div class="col-md-6">
              <div class="form-group">
                <label class="control-label col-md-5">labelClassName:</label>
                <div class="col-md-7">
                  <select class="form-control" v-model="labelClassName">
                    <option value="bg-white">bg-white</option>
                    <option value="bg-orange">bg-orange</option>
                    <option value="bg-gold">bg-gold</option>
                    <option value="bg-gray">bg-gray</option>
                    <option value="bg-lightpink">bg-lightpink</option>
                    <option value="bg-chocolate">bg-chocolate</option>
                    <option value="bg-tomato">bg-tomato</option>
                  </select>
                </div>
              </div>
            </div>
          </form>
        </div>
      </div>
      <p><br></p>
      <div class="text-center">
        <org-tree
          :data="data"
          :horizontal="horizontal"
          :collapsable="collapsable"
          :label-class-name="labelClassName"
          :render-content="renderContent"
          @on-expand="onExpand"
          @on-node-click="onNodeClick"
        >
        </org-tree>
      </div>
    </div>
  </div>
</template>

<script>
import OrgTree from './components/org-tree'

export default {
  name: 'app',
  components: {
    OrgTree
  },
  data () {
    return {
      data: {
        id: 0,
        label: 'XXX科技有限公司',
        children: [{
          id: 2,
          label: '产品研发部',
          children: [{
            id: 5,
            label: '研发-前端'
          }, {
            id: 6,
            label: '研发-后端'
          }, {
            id: 9,
            label: 'UI设计'
          }, {
            id: 10,
            label: '产品经理'
          }]
        }, {
          id: 3,
          label: '销售部',
          children: [{
            id: 7,
            label: '销售一部'
          }, {
            id: 8,
            label: '销售二部'
          }]
        }, {
          id: 4,
          label: '财务部'
        }, {
          id: 9,
          label: 'HR人事'
        }]
      },
      horizontal: false,
      collapsable: true,
      labelClassName: 'bg-white'
    }
  },
  methods: {
    renderContent (h, data) {
      return data.label
    },
    onExpand (data) {
      if ('expand' in data) {
        data.expand = !data.expand

        if (!data.expand && data.children) {
          this.collapse(data.children)
        }
      } else {
        this.$set(data, 'expand', true)
      }
    },
    onNodeClick (e, data) {
      alert(data.label)
    },
    collapse (list) {
      list.forEach(child => {
        if (child.expand) {
          child.expand = false
        }

        child.children && this.collapse(child.children)
      })
    }
  }
}
</script>

<style>
.bg-white {
  background-color: white;
}
.bg-orange {
  background-color: orange;
}
.bg-gold {
  background-color: gold;
}
.bg-gray {
  background-color: gray;
}
.bg-lightpink {
  background-color: lightpink;
}
.bg-chocolate {
  background-color: chocolate;
}
.bg-tomato {
  background-color: tomato;
}
</style>
