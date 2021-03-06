<template>
  <div class="card">
    <h5 class="card-header">
      {{
        showHeader
          ? ui.formatString(T.schoolRankOfTheMonthHeader, {
              count: rank ? rank.length : 0,
            })
          : ui.formatString(T.schoolRankRangeHeader, {
              lowCount: (page - 1) * length + 1,
              highCount: page * length,
            })
      }}
    </h5>
    <table class="table mb-0">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">{{ T.profileSchool }}</th>
          <th class="text-right" scope="col">{{ T.wordsScore }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-bind:key="index" v-for="(school, index) in rank">
          <th scope="row">
            {{ showHeader ? index + 1 : school.ranking || '' }}
          </th>
          <td class="text-truncate">
            <omegaup-countryflag
              v-bind:country="school.country_id"
            ></omegaup-countryflag>
            <a v-bind:href="`/schools/profile/${school.school_id}/`">{{
              school.name
            }}</a>
          </td>
          <td class="text-right">
            {{ school.score }}
          </td>
        </tr>
      </tbody>
    </table>
    <div class="card-footer" v-if="showHeader">
      <a href="/rank/schools/">{{ T.wordsSeeGeneralRanking }}</a>
    </div>
    <div class="card-footer" v-else="">
      <omegaup-common-paginator
        v-bind:pagerItems="pagerItems"
      ></omegaup-common-paginator>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../../../../sass/main.scss';
// FIXME: This prevents wrapping a table cell when the name of the school is too long.
// So, both tables (users rank and the current one) are perfectly aligned.
// Another solution should  be taken in the future.
.text-truncate {
  max-width: 250px;
}
</style>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';

import { omegaup } from '../../omegaup';
import { types } from '../../api_types';
import T from '../../lang';
import * as ui from '../../ui';
import CountryFlag from '../CountryFlag.vue';
import common_Paginator from '../common/Paginatorv2.vue';

@Component({
  components: {
    'omegaup-countryflag': CountryFlag,
    'omegaup-common-paginator': common_Paginator,
  },
})
export default class SchoolRank extends Vue {
  @Prop() page!: number;
  @Prop() length!: number;
  @Prop() showHeader!: boolean;
  @Prop() totalRows!: number;
  @Prop() rank!: omegaup.SchoolsRank[];
  @Prop() pagerItems!: types.PageItem[];

  T = T;
  ui = ui;
}
</script>
