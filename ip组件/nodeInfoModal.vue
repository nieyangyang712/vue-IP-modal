<template>
  <div>
    <!-- 新增节点 -->
    <Modal v-model="modal1" :mask-closable="false" draggable width="900" :title="title_name">
      <div>
        <Form ref="formAddNodeInfo" :model="formAddNodeInfo" :rules="addRules" label-position="right" :label-width="120">
          <Row :gutter="24">
            <Col span="11" style="padding-left: 0px;padding-right: 0px;">
              <FormItem label="节点IP地址1:" prop="ip">
                <IP :ipStr='IP_address' @getIP="ipchange"></IP>
              </FormItem>
              <FormItem label="节点IP地址2:" prop="ip">
                <ip-check v-model="formAddNodeInfo.ip"> </ip-check>
              </FormItem>
            </Col>
          </Row>
        </Form>
      </div>
    </Modal>
  </div>
</template>

<script>
import IPComponent from './IPComponent.vue';
import ipCheck from './ipCheck.vue';
export default {
  components: {'IP': IPComponent, 'ipCheck': ipCheck},
  data () {
    
    // 节点IP 验证
    const validateip = (rule, value, callback) => {
      if (value === '') {
        return callback(new Error('请输入设备IP！'))
      } else {
        return callback()
      }
    }
    
    return {
      // ip控件-----开始
      IP_address: '',
      erro_ip: true,
      ip: '',
      isWX: navigator.userAgent.toLowerCase().match(/MicroMessenger/i) === 'micromessenger',
      // ip控件-----结束

      title_name: 'IP组件',
      formAddNodeInfo: {
        ip: ''
      },
      // 判断新增节点输入框的规则
      addRules: {
        ip: [
          { required: true, type: 'string', message: 'ip不能为空！', trigger: 'blur' },
          { validator: validateip, trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // ip控件事件-----开始
    ipchange (ip) {
      if (ip) {
        this.formAddNodeInfo.ip = ip
      } else {
        this.formAddNodeInfo.ip = ""
      }
      // console.log('endIP',ip);
    },
    // ip控件事件-----结束
    // (服务节点) 取消
    cancel_nodeId() {
      this.modal1 = false;
    },
  },
  created() {},
  mounted(){
    this.modal1 = true;
  },
  destroyed () {
    this.$destroy();
  }
}
</script>