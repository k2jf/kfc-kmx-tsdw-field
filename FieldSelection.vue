<template>
  <div class="transfer-demo">
    <Transfer
      :data="fieldData"
      filterable
      :style="{width: '702px', margin: '0 auto'}"
      :list-style="{height: '400px', width: '300px'}"
      :target-keys="targetKeys"
      :titles="['未选字段', '已选字段']"
      :operations="['去除','选取']"
      @on-change="handleChange"
      @on-dblclick="handleChange">
    </Transfer>
  </div>
</template>
<script>
import Transfer from 'k2-transfer'

export default {
  name: 'FieldSelection',
  components: {
    Transfer
  },
  props: {
    fieldType: {
      type: String,
      default: 'gw_scada_7s'
    }
  },
  data () {
    return {
      fields: [],
      targetKeys: []
    }
  },
  computed: {
    fieldData () {
      return this.fields.map(f => {
        return {
          key: f.systemId.toString(),
          label: f.name
        }
      })
    }
  },
  mounted () {
    this.$axios.get('/kmx/data-service/v2/field-groups/' + this.fieldType).then(res => {
      this.fields = res.data.fieldGroup.fields
    })
  },
  methods: {
    handleChange (newTargetKeys) {
      this.targetKeys = newTargetKeys
      const selectedFeilds = this.fields.filter(
        f => newTargetKeys.includes(f.systemId.toString())
      )
      this.$emit('on-field-select-change', selectedFeilds)
    }
  }
}
</script>
<style lang="scss" scoped>
.transfer-demo {
  padding: 30px;
}
</style>
