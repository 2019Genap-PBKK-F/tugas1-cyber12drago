<template>
  <div>
    <div id="app" ref="spreadsheet"></div>
    <div>
        <input type="button" value="Add New Row" @click="() => spreadsheet.insertRow()" />
        <input type="button" value="Delete Selected Row" @click="() => spreadsheet.deleteRow()" />
    </div>
  </div>
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
import axios from 'axios'
export default {
  // name: 'App',
  data() {
    return {
      Mahasiswa: [],
      form: {
        Id: '',
        NRP: '',
        Nama: '',
        Tgl_lahir: ''
      }
    }
  },
  mounted() {
    let spreadsheet = jexcel(this.$el, this.jexcelOptions)
    Object.assign(this, { spreadsheet })
  },
  methods: {
    newRow() {
      axios.post('http://10.199.14.46:8020/api/mahasiswa', this.form).then(res => {
        console.log(res.data)
      })
    },
    updateRow(instance, cell, columns, row, value) {
      axios.get('http://10.199.14.46:8020/api/mahasiswa').then(res => {
        var index = Object.values(res.data[row])
        index[columns] = value
        console.log(index)
        axios.put('http://10.199.14.46:8020/api/mahasiswa' + index[0], {
          Id: index[0],
          NRP: index[1],
          Nama: index[2],
          Tgl_lahir: index[3]
        }).then(res => {
          console.log(res.data)
        })
      })
    },
    deleteRow(instance, row) {
      axios.get('http://10.199.14.46:8020/api/mahasiswa').then(res => {
        var index = Object.values(res.data[row])
        // console.log(index)
        console.log(row)
        axios.delete('http://10.199.14.46:8020/api/mahasiswa' + index[0])
      })
    }
  },
  computed: {
    jexcelOptions() {
      return {
        data: this.Mahasiswa,
        allowToolbar: true,
        url: 'http://10.199.14.46:8020/api/mahasiswa',
        onchange: this.updateRow,
        oninsertrow: this.newRow,
        ondeleterow: this.deleteRow,
        columns: [
          { type: 'numeric', title: 'id', width: '50px' },
          { type: 'text', title: 'NRP', width: '120px' },
          { type: 'text', title: 'Nama', width: '120px' },
          { type: 'text', title: 'Kelamin', width: '120px' },
          { type: 'numeric', title: 'Angkatan', width: '120px' },
          { type: 'text', title: 'Lahir', width: '120px' },
          { type: 'bit', title: 'Aktif', width: '120px' }
        ]
      }
    }
  }
}
</script>
