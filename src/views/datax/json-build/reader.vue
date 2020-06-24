<template>
  <div class="app-container">
    <RDBMSReader v-show="dataSource!=='hive' && dataSource!=='hbase' && dataSource!=='mongodb'" ref="rdbmsreader" @selectDataSource="showDataSource" @testHive="testHive" />
    <HiveReader v-show="dataSource==='hive'" ref="hivereader" @selectDataSource="showDataSource" @testHive="testHive" />
    <HBaseReader v-show="dataSource==='hbase'" ref="hbasereader" @selectDataSource="showDataSource" />
    <MongoDBReader v-show="dataSource==='mongodb'" ref="mongodbreader" @selectDataSource="showDataSource" />
  </div>
</template>

<script>
import RDBMSReader from './reader/RDBMSReader'
import HiveReader from './reader/HiveReader'
import HBaseReader from './reader/HBaseReader'
import MongoDBReader from './reader/MongoDBReader'
export default {
  name: 'Reader',
  components: { RDBMSReader, HiveReader, HBaseReader, MongoDBReader },
  data() {
    return {
      dataSource: '',
      dataSourceId: ''
    }
  },
  methods: {
    getData() {
      if (this.dataSource === 'hive') {
        return this.$refs.hivereader.getData()
      } else if (this.dataSource === 'hbase') {
        return this.$refs.hbasereader.getData()
      } else if (this.dataSource === 'mongodb') {
        return this.$refs.mongodbreader.getData()
      } else {
        return this.$refs.rdbmsreader.getData()
      }
    },
    showDataSource(dataSource, dataSourceId) {
      this.dataSource = dataSource
      this.dataSourceId = dataSourceId
      this.getData()
    },
    testHive(dataSource, dataSourceId) {
      this.$refs.hivereader.rDsChange(dataSourceId)
      return this.$refs.hivereader.getData()
    }
  }
}
</script>
