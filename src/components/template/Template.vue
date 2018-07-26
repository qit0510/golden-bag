<script>
  /* eslint-disable no-return-assign */
  /* eslint-disable no-param-reassign */
  import {createNamespacedHelpers} from 'vuex';
  import InputTr from './InputTr.vue';
const { mapActions, mapState } = createNamespacedHelpers('template');

export default{
  name: 'Template',
  components: {
    InputTr,
  },
  computed: {
    ...mapState([
      'currentTemplate',
    ]),
  },
  methods: {
    ...mapActions([
      'getTemplate',
      'updateTemplate',
    ]),
  },
  async created() {
    await this.getTemplate(this.$route.params.templateId);
    // 初始化
    this.currentTemplate.assessmentProjects.forEach((item) => {
      const opt = [];
      item.items.forEach((ele) => {
        opt.push({
          id: ele.id,
          title: ele.title,
          value: ele.score,
        });
      });
      this.proLists.push({
        id:item.id,
        name: item.title,
        options: opt,
        self_evaluation: '',
        direct_manager_score: '  ',
        remarks: '',
      });
    });
    this.currentTemplate.assessmentInputs.forEach((item) => {
      this.inputList.push({
        id: item.id,
        title: item.title,
        value: '',
      });
    });
  },
  render() {
    const proTrs = this.proLists.map((pro) => {
      const trs = pro.options.slice(1, pro.options.length).map((item, index) => {  // eslint-disable-line
        return (

          <tr>
            <td colspan="5"><InputTr value={item.title} on-blur={(v) => { this.updateTemplate({value: v,id:item.id,temp: 'project_item' }) } }
                                     on-input={v => item.title = v}/></td>
            <td class="editable">
              <InputTr value={item.value} on-blur={(v) => { this.updateTemplate({value: v,id:item.id,temp: 'score' }) } } on-input={v => item.value = v}/>
            </td>
          </tr>
        );
      });
      //temples/template_input/xx
      //temples/project/xx
      trs.unshift(<tr>
        <td rowspan={pro.options.length} className="editable">
          <InputTr type="textarea" value={pro.name} on-blur={(v) => { this.updateTemplate({value: v, id: pro.id,temp: 'project'}) } } on-input={v => pro.name = v}/>
        </td>
        <td colspan="5" className="editable">
          <InputTr value={pro.options[0].title} on-blur={(v) => { this.updateTemplate({value: v, id: pro.options[0].id,temp: 'project_item' }) } } on-input={v => pro.options[0].title = v}/>
        </td>
          <td class="editable">
            <InputTr value={pro.options[0].value} on-blur={(v) => { this.updateTemplate({ value: v, id: pro.options[0].id,temp: 'score' }) } } on-input={v => pro.options[0].value = v} />
          </td>
          <td rowspan={pro.options.length} class="editable">
            <InputTr value={pro.self_evaluation} on-input={v => pro.self_evaluation = v} />
          </td>
          <td rowspan={pro.options.length} class="editable">
            <InputTr value={pro.direct_manager_score} on-input={v => pro.direct_manager_score = v}/>
          </td>
          <td rowspan={pro.options.length} class="editable">
            <InputTr type="textarea" value={pro.remarks} on-input={v => pro.remarks = v} />
          </td>
        </tr>);
      return trs;
    });
    const inputTrs = this.inputList.map((inputItem) => {
      const titleTds = [<tr>
        <td colspan="10"><InputTr value={inputItem.title} on-blur={(v) => { this.updateTemplate({ value: v, id: inputItem.id,temp: 'template_input' }) } } on-input={v => inputItem.title = v}/></td>
      </tr>];
      titleTds.push(<tr colspan="10">
          <td class="editable input_td" colspan="10">
            <InputTr value={inputItem.value} on-blur={(v) => { this.updateTemplate({ value: v, id: inputItem.id,temp: 'template_input' }); } } on-input={v => inputItem.value = v}/>
          </td>
        </tr>);
      return titleTds;
    });
    return (
      <div class="template">
        <table class="table">
          <tr>
            <td colspan="10">上海容大数字技术有限公司</td>
          </tr>
          <tr>
            <td colspan="10">员工季度绩效考核表</td>
          </tr>
          <tr>
            <td colspan="10">注：本表适用于不带团队的员工填写</td>
          </tr>
          <tr>
            <td>姓名</td>
            <td class="editable">
              <InputTr value={ this.val } on-input={v => this.val = v} />
            </td>
            <td>部门</td>
            <td></td>
            <td>项目组</td>
            <td></td>
            <td>岗位</td>
            <td colspan="3"></td>
          </tr>
          <tr>
            <td>直接经理</td>
            <td></td>
            <td>间接经理</td>
            <td colspan="2"></td>
            <td>考核时间</td>
            <td></td>
            <td colspan="3">2018年第2季度</td>
          </tr>
          <tr>
            <td>考核项目</td>
            <td colspan="5">评分参考标准 </td>
            <td>分值</td>
            <td>自评得分</td>
            <td>直接经理评分</td>
            <td>备注</td>
          </tr>
          {proTrs}
          <tr>
            <td colspan="6">合记</td>
            <td class="editable">

              <InputTr value={this.val} on-input={v => this.val = v} />
            </td>
            <td class="editable">
              <InputTr value={this.val} on-input={v => this.val = v} />
            </td>
            <td class="editable">
              <InputTr value={this.val} on-input={v => this.val = v} />
            </td>
            <td class="editable">
              <InputTr value={this.val} on-input={v => this.val = v} />
            </td>
          </tr>
          <tr>
            <td colspan="10">工作总结、改进和工作目标计划</td>
          </tr>
          {inputTrs}
           <tr>
              <td colspan="4">间接经理审核意见</td>
              <td colspan="6" class="editable"><InputTr value={ this.val } on-input={v => this.val = v} /></td>
            </tr>
        </table>
      </div>
    );
  },
  data() {
    return {
      vale: 'ty',
      proLists: [],
      inputList: [],
    };
  },

};
</script>

<style lang="less" scoped>
.template{
  padding: 60px;
  background-color: #fff;
  .table,
  .table th,
  .table td {
    text-align: center;
    padding: 8px 3px;
    border: 1px solid rgb(219, 219, 219);
    border-collapse: collapse
  }
  .table{
    width: 100%;
  }
  .table td{
    min-width: 100px;
    background-color: #fff;
  }
  td.input_td{
    height: 120px;
  }
  .table td.editable{
    padding: 0!important;
  }
}
</style>

