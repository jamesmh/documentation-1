<template>
  <ClientOnly>
    <div>
      <el-form-item label="Number of nodes:" prop="nodesCount">
        <el-col :span="4">
          <el-input-number
                  v-model="nodesCount"
                  :min="minNodes"
                  :max="maxNodes"
          >
          </el-input-number>
        </el-col>
        <el-col :span="12" v-show="isCluster" class="form-help">
          We recommend <b>three nodes</b> for an HA cluster. The number of nodes should be odd.
        </el-col>
      </el-form-item>

      <transition-group name="slide">
        <ClusterNode
                v-for="item in nodes"
                :ref="`node-${item.index}`"
                :key="`node-${item.index}`"
                :node-index="item.index"
                :show-int-ip="showIntIp"
                :hostname-required="hostnameRequired"
        />
      </transition-group>

    </div>
  </ClientOnly>
</template>

<script>
import ClusterNode from "./ClusterNode";
import nodesStore from "../../domain/nodes";

export default {
    name:       "ClusterNodes",
    components: {ClusterNode},
    props:      {
        showIntIp:        Boolean,
        isCluster:        Boolean,
        hostnameRequired: Boolean
    },
    computed:   {
        nodes:      () => nodesStore.nodes,
        minNodes:   () => nodesStore.minNodes,
        maxNodes:   () => nodesStore.maxNodes,
        nodesCount: nodesStore.property("nodesCount")
    },
    methods:    {
        validate() {
            this.nodes.forEach(node => this.$refs[`node-${node.index}`][0].validate());
        }
    }
}
</script>
