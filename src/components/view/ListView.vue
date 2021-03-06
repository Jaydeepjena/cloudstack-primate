// Licensed to the Apache Software Foundation (ASF) under one
// or more contributor license agreements.  See the NOTICE file
// distributed with this work for additional information
// regarding copyright ownership.  The ASF licenses this file
// to you under the Apache License, Version 2.0 (the
// "License"); you may not use this file except in compliance
// with the License.  You may obtain a copy of the License at
//
//   http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing,
// software distributed under the License is distributed on an
// "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
// KIND, either express or implied.  See the License for the
// specific language governing permissions and limitations
// under the License.

<template>
  <a-table
    size="small"
    :loading="loading"
    :columns="columns"
    :dataSource="items"
    :rowKey="record => record.id || record.name"
    :pagination="false"
    :rowSelection="['vm-tbd', 'event-tbd', 'alert-tbd'].includes($route.name) ? {selectedRowKeys: selectedRowKeys, onChange: onSelectChange} : null"
    :rowClassName="getRowClassName"
    style="overflow-y: auto"
  >
    <template slot="footer">
      <span v-if="hasSelected">
        {{ `Selected ${selectedRowKeys.length} items` }}
      </span>
    </template>

    <!--
    <div slot="expandedRowRender" slot-scope="resource">
      <info-card :resource="resource" style="margin-left: 0px; width: 50%">
        <div slot="actions" style="padding-top: 12px">
          <a-tooltip
            v-for="(action, actionIndex) in $route.meta.actions"
            :key="actionIndex"
            placement="bottom">
            <template slot="title">
              {{ $t(action.label) }}
            </template>
            <a-button
              v-if="action.api in $store.getters.apis && action.dataView &&
                ('show' in action ? action.show(resource, $store.getters.userInfo) : true)"
              :icon="action.icon"
              :type="action.icon === 'delete' ? 'danger' : (action.icon === 'plus' ? 'primary' : 'default')"
              shape="circle"
              style="margin-right: 5px; margin-top: 12px"
              @click="$parent.execAction(action)"
            >
            </a-button>
          </a-tooltip>
        </div>
      </info-card>
    </div>
    -->

    <a slot="name" slot-scope="text, record" href="javascript:;">
      <div style="min-width: 120px">
        <span v-if="$route.path.startsWith('/project')" style="margin-right: 5px">
          <a-button type="dashed" size="small" shape="circle" icon="login" @click="changeProject(record)" />
        </span>
        <os-logo v-if="record.ostypename" :osName="record.ostypename" size="1x" style="margin-right: 5px" />
        <console :resource="record" size="small" style="margin-right: 5px" />
        <router-link :to="{ path: $route.path + '/' + record.id }" v-if="record.id">{{ text }}</router-link>
        <router-link :to="{ path: $route.path + '/' + record.name }" v-else>{{ text }}</router-link>
      </div>
    </a>
    <a slot="displayname" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: $route.path + '/' + record.id }">{{ text }}</router-link>
    </a>
    <a slot="username" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: $route.path + '/' + record.id }">{{ text }}</router-link>
    </a>
    <a slot="ipaddress" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: $route.path + '/' + record.id }">{{ text }}</router-link>
      <span v-if="record.issourcenat">
        &nbsp;
        <a-tag>source-nat</a-tag>
      </span>
    </a>
    <a slot="publicip" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: $route.path + '/' + record.id }">{{ text }}</router-link>
    </a>
    <a slot="traffictype" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: $route.path + '/' + record.id + '?physicalnetworkid=' + record.physicalnetworkid }">{{ text }}</router-link>
    </a>
    <a slot="vmname" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/vm/' + record.virtualmachineid }">{{ text }}</router-link>
    </a>
    <template slot="state" slot-scope="text">
      <status :text="text ? text : ''" displayText />
    </template>
    <template slot="allocationstate" slot-scope="text">
      <status :text="text ? text : ''" displayText />
    </template>
    <template slot="resourcestate" slot-scope="text">
      <status :text="text ? text : ''" displayText />
    </template>
    <template slot="powerstate" slot-scope="text">
      <status :text="text ? text : ''" displayText />
    </template>
    <template slot="agentstate" slot-scope="text">
      <status :text="text ? text : ''" displayText />
    </template>
    <a slot="guestnetworkname" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/guestnetwork/' + record.guestnetworkid }">{{ text }}</router-link>
    </a>
    <a slot="vpcname" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/vpc/' + record.vpcid }">{{ text }}</router-link>
    </a>
    <a slot="hostname" slot-scope="text, record" href="javascript:;">
      <router-link v-if="record.hostid" :to="{ path: '/host/' + record.hostid }">{{ text }}</router-link>
      <router-link v-else-if="record.hostname" :to="{ path: $route.path + '/' + record.id }">{{ text }}</router-link>
      <span v-else>{{ text }}</span>
    </a>
    <a slot="clustername" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/cluster/' + record.clusterid }">{{ text }}</router-link>
    </a>
    <a slot="podname" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/pod/' + record.podid }">{{ text }}</router-link>
    </a>
    <a slot="account" slot-scope="text, record" href="javascript:;">
      <router-link :to="{ path: '/account/' + record.accountid }" v-if="record.accountid">{{ text }}</router-link>
      <router-link :to="{ path: '/account', query: { name: record.account, domainid: record.domainid } }" v-else>{{ text }}</router-link>
    </a>
    <span slot="domain" slot-scope="text, record" href="javascript:;">
      <router-link v-if="record.domainid && !record.domainid.includes(',') && $router.resolve('/domain/' + record.domainid).route.name !== '404'" :to="{ path: '/domain/' + record.domainid }">{{ text }}</router-link>
      <span v-else>{{ text }}</span>
    </span>
    <a slot="zone" slot-scope="text, record" href="javascript:;">
      <router-link v-if="record.zoneid && !record.zoneid.includes(',') && $router.resolve('/zone/' + record.zoneid).route.name !== '404'" :to="{ path: '/zone/' + record.zoneid }">{{ text }}</router-link>
      <span v-else>{{ text }}</span>
    </a>
    <span slot="zonename" slot-scope="text, record">
      <router-link v-if="$router.resolve('/zone/' + record.zoneid).route.name !== '404'" :to="{ path: '/zone/' + record.zoneid }">{{ text }}</router-link>
      <span v-else>{{ text }}</span>
    </span>

    <div slot="order" slot-scope="text, record" class="shift-btns">
      <a-tooltip placement="top">
        <template slot="title">Move to top</template>
        <a-button
          shape="round"
          @click="moveItemTop(record)"
          class="shift-btn">
          <a-icon type="double-left" class="shift-btn shift-btn--rotated" />
        </a-button>
      </a-tooltip>
      <a-tooltip placement="top">
        <template slot="title">Move to bottom</template>
        <a-button
          shape="round"
          @click="moveItemBottom(record)"
          class="shift-btn">
          <a-icon type="double-right" class="shift-btn shift-btn--rotated" />
        </a-button>
      </a-tooltip>
      <a-tooltip placement="top">
        <template slot="title">Move up one row</template>
        <a-button shape="round" @click="moveItemUp(record)" class="shift-btn">
          <a-icon type="caret-up" class="shift-btn" />
        </a-button>
      </a-tooltip>
      <a-tooltip placement="top">
        <template slot="title">Move down one row</template>
        <a-button shape="round" @click="moveItemDown(record)" class="shift-btn">
          <a-icon type="caret-down" class="shift-btn" />
        </a-button>
      </a-tooltip>
    </div>

    <template slot="value" slot-scope="text, record">
      <a-input
        v-if="editableValueKey === record.key"
        :defaultValue="record.value"
        v-model="editableValue"
        @keydown.esc="editableValueKey = null"
        @pressEnter="saveValue(record)">
      </a-input>
      <div v-else style="width: 200px; word-break: break-all">
        {{ text }}
      </div>
    </template>
    <template slot="actions" slot-scope="text, record">
      <a-button
        shape="circle"
        v-if="editableValueKey !== record.key"
        icon="edit"
        @click="editValue(record)" />
      <a-button
        shape="circle"
        @click="saveValue(record)"
        v-if="editableValueKey === record.key" >
        <a-icon type="check-circle" theme="twoTone" twoToneColor="#52c41a" />
      </a-button>
      <a-button
        shape="circle"
        size="default"
        @click="editableValueKey = null"
        v-if="editableValueKey === record.key" >
        <a-icon type="close-circle" theme="twoTone" twoToneColor="#f5222d" />
      </a-button>
    </template>
  </a-table>
</template>

<script>
import { api } from '@/api'
import Console from '@/components/widgets/Console'
import OsLogo from '@/components/widgets/OsLogo'
import Status from '@/components/widgets/Status'
import InfoCard from '@/components/view/InfoCard'

export default {
  name: 'ListView',
  components: {
    Console,
    OsLogo,
    Status,
    InfoCard
  },
  props: {
    columns: {
      type: Array,
      required: true
    },
    items: {
      type: Array,
      required: true
    },
    loading: {
      type: Boolean,
      default: false
    }
  },
  inject: ['parentFetchData', 'parentToggleLoading'],
  data () {
    return {
      selectedRowKeys: [],
      editableValueKey: null,
      editableValue: ''
    }
  },
  computed: {
    hasSelected () {
      return this.selectedRowKeys.length > 0
    }
  },
  methods: {
    getRowClassName (record, index) {
      if (index % 2 === 0) {
        return 'light-row'
      }
      return 'dark-row'
    },
    onSelectChange (selectedRowKeys) {
      console.log('selectedRowKeys changed: ', selectedRowKeys)
      this.selectedRowKeys = selectedRowKeys
    },
    changeProject (project) {
      this.$store.dispatch('SetProject', project)
      this.$store.dispatch('ToggleTheme', project.id === undefined ? 'light' : 'dark')
      this.$message.success(`Switched to "${project.name}"`)
      this.$router.push({ name: 'dashboard' })
    },
    saveValue (record) {
      api('updateConfiguration', {
        name: record.name,
        value: this.editableValue
      }).then(json => {
        this.editableValueKey = null

        this.$message.success('Setting Updated: ' + record.name)
        if (json.updateconfigurationresponse &&
          json.updateconfigurationresponse.configuration &&
          !json.updateconfigurationresponse.configuration.isdynamic &&
          ['Admin'].includes(this.$store.getters.userInfo.roletype)) {
          this.$notification.warning({
            message: 'Status',
            description: 'Please restart your management server(s) for your new settings to take effect.'
          })
        }
      }).catch(error => {
        console.error(error)
        this.$message.error('There was an error saving this setting.')
      })
        .finally(() => {
          this.$emit('refresh')
        })
    },
    editValue (record) {
      this.editableValueKey = record.key
      this.editableValue = record.value
    },
    handleUpdateOrder (id, index) {
      this.parentToggleLoading()
      let apiString = ''
      switch (this.$route.name) {
        case 'template':
          apiString = 'updateTemplate'
          break
        case 'iso':
          apiString = 'updateIso'
          break
        case 'zone':
          apiString = 'updateZone'
          break
        case 'computeoffering':
        case 'systemoffering':
          apiString = 'updateServiceOffering'
          break
        case 'diskoffering':
          apiString = 'updateDiskOffering'
          break
        case 'networkoffering':
          apiString = 'updateNetworkOffering'
          break
        case 'vpcoffering':
          apiString = 'updateVPCOffering'
          break
        default:
          apiString = 'updateTemplate'
      }

      api(apiString, {
        id,
        sortKey: index
      }).catch(error => {
        console.error(error)
      }).finally(() => {
        this.parentFetchData()
        this.parentToggleLoading()
      })
    },
    moveItemUp (record) {
      const data = this.items
      const index = data.findIndex(item => item.id === record.id)
      if (index === 0) return

      data.splice(index - 1, 0, data.splice(index, 1)[0])

      data.forEach((item, index) => {
        this.handleUpdateOrder(item.id, index + 1)
      })
    },
    moveItemDown (record) {
      const data = this.items
      const index = data.findIndex(item => item.id === record.id)
      if (index === data.length - 1) return

      data.splice(index + 1, 0, data.splice(index, 1)[0])

      data.forEach((item, index) => {
        this.handleUpdateOrder(item.id, index + 1)
      })
    },
    moveItemTop (record) {
      const data = this.items
      const index = data.findIndex(item => item.id === record.id)
      if (index === 0) return

      data.unshift(data.splice(index, 1)[0])

      data.forEach((item, index) => {
        this.handleUpdateOrder(item.id, index + 1)
      })
    },
    moveItemBottom (record) {
      const data = this.items
      const index = data.findIndex(item => item.id === record.id)
      if (index === data.length - 1) return

      data.push(data.splice(index, 1)[0])

      data.forEach((item, index) => {
        this.handleUpdateOrder(item.id, index + 1)
      })
    }
  }
}
</script>

<style scoped>
/deep/ .ant-table-thead {
  background-color: #f9f9f9;
}

/deep/ .ant-table-small > .ant-table-content > .ant-table-body {
  margin: 0;
}

/deep/ .light-row {
  background-color: #fff;
}

/deep/ .dark-row {
  background-color: #f9f9f9;
}
</style>

<style scoped lang="scss">
  .shift-btns {
    display: flex;
  }
  .shift-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 20px;
    height: 20px;
    font-size: 12px;

    &:not(:last-child) {
      margin-right: 5px;
    }

    &--rotated {
      font-size: 10px;
      transform: rotate(90deg);
    }

  }
</style>
