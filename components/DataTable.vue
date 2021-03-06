<template>
  <data-view :title="title" :title-id="titleId" :date="date">
    <template v-slot:button>
      <span />
    </template>
    <v-data-table
      :ref="'displayedTable'"
      :headers="chartData.headers"
      :items="chartData.datasets"
      :height="240"
      fixed-header
      :mobile-breakpoint="0"
      :custom-sort="customSort"
      :footer-props="{
        'items-per-page-options': [15, 30, 50, 100, 200, 300, -1],
        'items-per-page-text': $t('1ページ当たり')
      }"
      class="cardTable"
    >
      <template v-slot:body="{ items }">
        <tbody>
          <tr v-for="item in items" :key="item.text">
            <th class="text-start" scope="row">{{ item['確定日'] }}</th>
            <td class="text-start">{{ item['居住地'] }}</td>
            <td class="text-start">{{ item['年代'] }}</td>
            <td class="text-start">{{ item['性別'] }}</td>
          </tr>
        </tbody>
      </template>
      <template slot="footer.page-text" slot-scope="props">
        {{
          $t('{itemsLength} 項目中 {pageStart} - {pageStop}', {
            itemsLength: props.itemsLength,
            pageStart: props.pageStart,
            pageStop: props.pageStop
          })
        }}
      </template>
    </v-data-table>
    <!-- <div class="note">
      {{ $t('※退院には、死亡退院を含む') }}
    </div>
    <div class="note">
      {{ $t('※入院調整中には、確認中も含む') }}
    </div> -->
    <template v-slot:infoPanel>
      <data-view-basic-info-panel
        :l-text="info.lText"
        :s-text="info.sText"
        :unit="info.unit"
      />
    </template>
    <template v-slot:footer>
      <open-data-link :url="url" />
    </template>
  </data-view>
</template>

<style lang="scss">
.cardTable {
  &.v-data-table {
    th {
      padding: 8px 10px;
      height: auto;
      border-bottom: 1px solid $gray-4;
      white-space: nowrap;
      color: $gray-2;
      font-size: 12px;

      &.text-center {
        text-align: center;
      }
    }

    tbody {
      tr {
        color: $gray-1;
        th {
          font-weight: normal;
        }
        td {
          padding: 8px 10px;
          height: auto;
          font-size: 12px;

          &.text-center {
            text-align: center;
          }
        }

        &:nth-child(odd) {
          th,
          td {
            background: rgba($gray-4, 0.3);
          }
        }
      }
    }
    .v-select {
      margin-left: 10px;
    }
    &:focus {
      outline: dotted $gray-3 1px;
    }
  }
  .v-data-table__wrapper {
    box-shadow: 0 -20px 12px -12px #0003 inset;
  }
  .v-data-footer {
    @include font-size(12);
    &__pagination {
      margin-left: 0;
      margin-right: 5px;
    }
  }
  .v-data-footer__select .v-select__selections .v-select__selection--comma {
    font-size: 1.2rem;
  }
}
.note {
  padding: 8px;
  font-size: 12px;
  color: $gray-3;
}
.v-menu__content {
  width: 60px;
  .v-list-item {
    padding: 0 8px;
  }
}
.v-list-item__title {
  font-size: 1.5rem;
}
</style>

<script lang="ts">
import Vue from 'vue'
import DataView from '@/components/DataView.vue'
import DataViewBasicInfoPanel from '@/components/DataViewBasicInfoPanel.vue'
import OpenDataLink from '@/components/OpenDataLink.vue'

export default Vue.extend({
  components: { DataView, DataViewBasicInfoPanel, OpenDataLink },
  props: {
    title: {
      type: String,
      default: ''
    },
    titleId: {
      type: String,
      default: ''
    },
    chartData: {
      type: Object,
      default: () => {}
    },
    date: {
      type: String,
      default: ''
    },
    info: {
      type: Object,
      default: () => {}
    },
    url: {
      type: String,
      default: ''
    }
  },
  mounted() {
    const vTables = this.$refs.displayedTable as Vue
    const vTableElement = vTables.$el
    const tables = vTableElement.querySelectorAll('table')

    tables.forEach((table: HTMLElement) => {
      table.setAttribute('tabindex', '0')
    })
  }
})
</script>
