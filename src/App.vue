<template>
  <div id="app">
    <div class="columns">
      <b-field class="column is-4" label="Số m2 cần">
        <b-input type="number" v-model="meters" @input="(i) => onChange(i, 'meters')">
        </b-input>
      </b-field> 
      <b-field class="column is-4" label="Số thùng cần">
        <b-input type="number" v-model="boxes"  @input="(i) => onChange(i, 'boxes')">
        </b-input>
      </b-field>
    </div>
    <div class="columns is-mobile">
      <b-field class="column is-full" label="Chọn kích thước">
        <b-select
          @input="onChange"
          v-model="tileSizeId"
          placeholder="Chọn kích thước gạch">
            <option
              v-for="option in tileSizeOption"
              :value="option.id"
              :key="option.id">
              {{ option.name }}
            </option>
        </b-select>
      </b-field>
      <span class="column is-full">Số m&#178; thực tế: {{ realMeters }} ( m&#178; )</span>
      <span class="column is-full">Số thùng thực tế: {{ realBoxes }} ( thùng )</span>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      meters: null,
      realMeters: 0,
      boxes: null,
      realBoxes: 0,
      tileSizeId: 1,
      currentInputField: 'meters',
      tileSizeOption: [
        {id: 1, name: '30X60 (8 viên trong 1 thùng)', value: [30,60], tilePerBox: 8},
        {id: 2, name: '40X40 (6 viên trong 1 thùng)', value: [40,40], tilePerBox: 6},
      ]
    }
  },
  methods: {
    onChange(value, field = null) {
      if (field) {
        this.currentInputField = field
      }
      this.convert()
    },
    convert() {
      if (!this.tileSizeId) return;
      let tileSize = this.tileSizeOption.find(size => size.id === this.tileSizeId)
      let tileSquare = tileSize.value[0] * tileSize.value[1] / 10000
      console.log('tileSquare', tileSquare)
      let boxSquare = tileSquare *tileSize.tilePerBox
      console.log('boxSquare', boxSquare)
      if (this.currentInputField === 'meters') {
        let boxes = Math.ceil(this.meters / boxSquare)
        this.realMeters = boxes * boxSquare
        this.realBoxes = boxes
        this.$nextTick(() => {
          this.boxes = boxes
        })
      } else {
        let meters = this.boxes * boxSquare
        this.realMeters = meters
        this.realBoxes = this.boxes
        this.$nextTick(() => {
          this.meters = meters
        })

      }
    }
  },
 
}
</script>

<style lang="scss">
.columns {
  padding: 10px 40px;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
</style>
