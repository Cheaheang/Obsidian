```vue
<template>

  <PanelCreate

    v-if="parentReady"

    :key="panelKey"

    :defaultHiddenPanelHead="true"

    :pageName="'Deal'"

    :folderName="'CRM'"

    :indexPageUrl="'/app/crm/deal'"

    :modelId="form_data.id"

    :auditColWidth="12"

    class="deal-page"

    :workflowInstance="'crm_deals'"

  >

    <template #actions>

    <b-button

      variant="warning"

      class="mr-2"

      @click="Submit_Form('save')"

    >

      <i class="bi bi-save"></i> Save

    </b-button>

  </template>

  

    <template #content-body>

      <validation-observer ref="Validate_Form_Data" v-if="!isLoading">

        <b-form>

          <button

  v-if="isSummaryCollapsed"

  class="summary-fab"

  @click="isSummaryCollapsed = false"

>

  <i class="bi bi-chevron-left"></i>

  <span>Summary</span>

</button>  

  

          <!-- MAIN GRID -->

          <b-row>

            <!-- LEFT -->

            <!-- <b-col :md="isSummaryCollapsed ? 12 : 9" class="mb-3 transition-col"> -->

            <b-col md="9" class="mb-3 main-col"   :class="{ 'mx-auto': isSummaryCollapsed }" >

              <b-card no-body>

                <b-tabs v-model="activeTab" ref="dealTabs" card lazy pills nav-class="qtabs">

                  <b-tab value="details">

                    <template #title><i class="bi bi-person-lines-fill mr-1"></i> Details</template>

                    <b-card-body>

  

                      <b-card class="mt-3 flat-card">

  

                      <!-- Deal Information -->

                      <div>

                        <b-card-title>

                          <div class="d-flex align-items-center mt-2">

                            <span class="tite-pill">

                              <i class="bi bi-briefcase-fill"></i>

                            </span>

                            <div class="ml-2">

                              <h6 class="mb-0">Deal Information</h6>

                            </div>

                          </div>

                        </b-card-title>

                        <b-row>

                          <FormInput name="object_name" label="Deal Name" v-model="form_data.object_name"

                            columnWidth="12" :rules="{ required: true }" label-position="top" />

  

<FormSelect

  v-model="pipelineIdProxy"

  name="pipeline_id"

  label="Pipeline"

  :options="pipelines"

  columnWidth="4"

  label-position="top"

  placeholder="Select pipeline"

  :rules="{ required: true }"

/>

  

<FormSelect

  v-model="form_data.connectivity_type"

  name="connectivity_type"

  label="Connectivity Type"

  :options="connectivityOptions"

  columnWidth="4"

  label-position="top"

  placeholder="Select type"

  :disabled="pipelineIdProxy === null"

  :rules="{ required: true }"

/>

                          <FormSelect v-model="form_data.vertical_id" name="vertical_id" label="Vertical"

                            :options="verticals" columnWidth="4" label-position="top" placeholder="Select vertical"

                            :rules="{ required: true }" @input="verticalChanged" />

  

                          <FormSelect v-model="form_data.sub_vertical_id" name="sub_vertical_id" label="Sub Vertical"

                            :options="subVerticalsByVertical" columnWidth="4" label-position="top"

                            placeholder="Select sub vertical" :rules="{ required: true }" />

  

                          <FormSelect v-model="form_data.source_id" name="source_id" label="Source"

                            :options="dealSources" columnWidth="4" label-position="top" placeholder="Select source"

                            :rules="{ required: true }" />

  

                          <FormDate name="start_date" label="Start Date" v-model="form_data.start_date"

                            label-position="top" columnWidth="4" :rules="{ required: true }" />

  

                          <FormDate name="end_date" label="End Date" v-model="form_data.end_date" label-position="top"

                            columnWidth="4" :rules="{ required: false }" v-show="false" />

  

                          <FormDate name="expected_close_date" label="Expected Close"

                            v-model="form_data.expected_close_date" label-position="top" columnWidth="4"

                            :rules="{ required: true }" />

  

                          <!-- Currency fields use b-form-input for live formatting -->

                          <b-col md="4" class="mb-2">

                            <label>MRC (VAT incl.)</label>

                            <b-form-input v-model="mrcInput" @blur="commitCurrency('mrc')"

                              @input="mrcInput = formatCurrencyTyping(mrcInput)" inputmode="decimal" readonly />

                          </b-col>

  

                          <b-col md="4" class="mb-2">

                            <label>NRC (VAT incl.)</label>

                            <b-form-input v-model="nrcInput" @blur="commitCurrency('nrc')"

                              @input="nrcInput = formatCurrencyTyping(nrcInput)" inputmode="decimal" readonly />

                          </b-col>

  

                          <FormSelect v-model="form_data.term_months" name="term_months" label="Service Period (Months)"

                            :options="servicePeriods" columnWidth="4" label-position="top"

                            placeholder="Select Service Period (Months)" />

  

                          <FormSelect v-model="form_data.new_sale_contract_month" name="new_sale_contract_month"

                            label="New Sale Contract (Months)" :options="newSaleContracts" columnWidth="4"

                            label-position="top" placeholder="Select New Sale Contract" :rules="{ required: true }" />

  

                          <FormInput v-model.number="form_data.new_sale_contract_month" name="new_sale_contract_month" label="New Sale Contract (Months)"

                              validationType="number"

                              type="number" min="0"

                              step="1"

                              inputmode="numeric"

                              pattern="[0-9]*"

                              columnWidth="4"

                              label-position="top"

                              :rules="{ required: true }"

                            />

  

                          <b-col md="4" class="mb-2">

                            <label>Forecast (VAT incl.)</label>

                            <b-form-input v-model="forecastInput" @blur="commitCurrency('forecast')"

                              @input="forecastInput = formatCurrencyTyping(forecastInput)" inputmode="decimal"

                              readonly />

                          </b-col>

  

                          <FormDate name="previous_isp_contract_end_date" label="Prev ISP End Date"

                            v-model="form_data.previous_isp_contract_end_date" label-position="top" columnWidth="4"

                            :rules="{ required: false }" />

  

                          <FormSelect v-model="form_data.sales_division_id" name="sales_division_id"

                            label="Sales Division" :options="saleDivisions" columnWidth="4" label-position="top"

                            placeholder="Select division" :rules="{ required: true }" />

  

                          <FormSelect v-model="form_data.deal_type_id" name="deal_type_id" label="Deal Type"

                            :options="dealTypes" columnWidth="4" label-position="top" placeholder="Select deal type"

                            :rules="{ required: true }" />

  

                          <FormSelect v-show="false" v-model="form_data.branch_id" name="branch_id" label="Branch"

                            :options="branchs" columnWidth="4" label-position="top" placeholder="Select branch"

                            :rules="{ required: false }" />

  

                          <FormSelect v-model="form_data.technical_pre_sale_id" name="technical_pre_sale_id"

                            label="Technical Pre-Sale"

                            :options="technicalPreSales" columnWidth="4"

                            label-position="top"

                            placeholder="Select pre-sale"

                            :rules="technicalPresaleRules" />

  

                          <FormInput v-model="form_data.existing_cid" name="existing_cid" label="Existing CID"

                            validationType="integer" columnWidth="4" label-position="top" :rules="{ required: true }" />

  

                          <FormSelect v-model="form_data.previous_isp_id" name="previous_isp_id" label="Previous ISP"

                            :options="previousISPs" columnWidth="4" label-position="top"

                            placeholder="Select Previous ISP" />

  
  

                          <FormInput v-model="form_data.last_mile_price" name="last_mile_price" label="Last Mile Price"

                            validationType="decimal" columnWidth="4" label-position="top" />

  

                          <FormInput v-model="form_data.voucher_code" name="voucher_code" label="Voucher Code"

                            columnWidth="4" label-position="top" v-show="false" />

  

                          <FormInput v-model="form_data.voucher_comment" name="voucher_comment" label="Voucher Comment"

                            columnWidth="4" label-position="top" v-show="false" />

  
  

                          <FormInput v-model="form_data.last_mile" name="last_mile" label="Last Mile Distance (m)"

                            validationType="decimal" columnWidth="4" label-position="top" />

                        </b-row>

                      </div>

  

                      <div>

                          <div class="d-flex align-items-center mb-4 mt-4">

                            <span class="tite-pill">

                              <i class="bi bi-geo-alt-fill"></i>

                            </span>

                            <div class="ml-2">

                              <h6 class="mb-0">Service Address</h6>

                            </div>

                          </div>

                        <b-row>

                        <b-col md="4">

                          <CompanySelectCard

                            v-model="form_data.company_id"

                            :contact-id.sync="form_data.contact_id"

                            :address-override.sync="useCustomServiceAddress"

                            :service-address.sync="serviceAddress"

                            :deal-name.sync="form_data.object_name"

                          />

                        </b-col>

  
  

                          <FormSelect name="province_id" label="Province" columnWidth="4" label-position="top"

                            v-model="form_data.province_id" :options="provinces" :rules="{ required: true }"

                            placeholder="Select province" @input="provinceChanged" />

  

                          <FormSelect name="district_id" label="District" columnWidth="4" label-position="top"

                            v-model="form_data.district_id" :options="districtsByProvince" :rules="{ required: true }"

                            placeholder="Select district" @input="districtChanged" />

  

                          <FormSelect name="commune_id" label="Commune" columnWidth="4" v-model="form_data.commune_id"

                            label-position="top" :options="communesByDistrict" :rules="{ required: true }"

                            placeholder="Select commune" @input="communeChanged" />

  

                          <FormSelect name="village_id" label="Village" columnWidth="4" v-model="form_data.village_id"

                            label-position="top" :options="villagesByCommune" :rules="{ required: false }"

                            placeholder="Select village" />

  

                          <FormInput name="house" label="House Number" v-model="form_data.house" columnWidth="4"

                            label-position="top" placeholder="House number" :rules="{ required: true }" />

  

                          <FormInput name="street" label="Street" v-model="form_data.street" columnWidth="4"

                            label-position="top" placeholder="Street" :rules="{ required: true }" />

  

                          <FormInput name="gps_location" label="Latitude, Longitude" v-model="form_data.gps_location"

                            columnWidth="4" label-position="top" placeholder="11.559250568826307, 104.92507996923004"

                            :rules="{ required: true }" />

  

                          <FormSelect name="borey_id" label="Borey" columnWidth="4" v-model="form_data.borey_id"

                            label-position="top" :options="bories" :rules="{ required: false }"

                            placeholder="Select borey" />

                        </b-row>

                      </div>

                    </b-card>

                    </b-card-body>

                  </b-tab>

  

                  <b-tab value="products">

                    <template #title>

                      <i class="bi bi-box2 mr-1"></i> Products

                      <span v-if="items.length" class="tab-badge">{{ items.length }}</span>

                    </template>

                    <b-card-body>

                      <div class="d-flex justify-content-between align-items-center mb-2">

                        <h6 class="mb-0"><i class="bi bi-box2-fill"></i> Product Information</h6>

                        <!-- Actions (anywhere on page) -->

                        <b-button size="sm" variant="success" @click="openProductPickerBulk">

                          <i class="bi bi-plus-lg"></i> Add Product

                        </b-button>

                      </div>

  
  

                      <ProductPickerTable ref="productPicker" selectionMode="multiple" @picked="onProductPicked" />

  

                      <b-table :items="items" :fields="fieldsWithActions" bordered small responsive="md"

                        sticky-header="55vh" thead-class="custom-table-header" class="rounded overflow-hidden">

  

                        <!-- Control column widths -->

                        <template #table-colgroup>

                          <col style="width:30%"> <!-- product -->

                          <col style="width:10%"> <!-- price (was 3%) -->

                          <col style="width:5%"> <!-- qty -->

                          <col style="width:14%"> <!-- uom -->

                          <col style="width:12%"> <!-- discount % -->

                          <col style="width:12%"> <!-- discount $ -->

                          <col style="width:10%"> <!-- tax -->

                          <col style="width:12%"> <!-- sub total -->

                          <col style="width:3%"> <!-- actions -->

                        </template>

  

                        <template #cell(product)="ctx">

                          <div class="font-weight-600">{{ ctx.item.product }}</div>

                          <div class="small text-muted">{{ ctx.item.period }}</div>

                        </template>

  

                        <template #cell(uom)="ctx">

                             <v-select

                              v-model="ctx.item.uom"

                              :options="uomOptions"

                              label="label"

                              :reduce="o => o.value"

                              :clearable="false"

                              :searchable="false"

                              :append-to-body="true"

                              class="table-input uom-select"

                            />

                          <!-- <FormSelect

                            v-model="ctx.item.uom"

                            name="uom"

                            :options="uomOptions"

                            placeholder="Select"

                          /> -->

                        </template>

  
  

                        <template #cell(price)="ctx">

                          <b-form-input class="num-input" v-model.number="ctx.item.price" type="number" min="0"

                            step="0.01" @input="updateRow(ctx.index)" />

                        </template>

  

                        <template #cell(qty)="ctx">

                          <b-form-input class="num-input" v-model.number="ctx.item.qty" type="number" min="1" step="1"

                            @input="updateRow(ctx.index)" />

                        </template>

  

                        <!-- <template #cell(discount_percent)="ctx">

                          <b-input-group>

                            <b-form-input

                              class="num-input"

                              v-model.number="ctx.item.discount_percent"

                              type="number"

                              min="0"

                              max="100"

                              step="0.01"

                              @input="updateRow(ctx.index)"

                            />

                            <b-input-group-append>

                              <b-input-group-text>%</b-input-group-text>

                            </b-input-group-append>

                          </b-input-group>

                        </template> -->

  

                        <template #cell(discount_percent)="ctx">

                          <b-input-group class="num-group flex-nowrap">

                            <b-form-input class="num-input" v-model.number="ctx.item.discount_percent" type="number"

                              min="0" max="100" step="0.01" @input="updateRow(ctx.index)" />

                            <b-input-group-append is-text>%</b-input-group-append>

                          </b-input-group>

                        </template>

  
  

                        <template #cell(discount_amount)="ctx">

                          <b-form-input :value="formatNumber(ctx.item.discount_amount)" readonly />

                        </template>

  

                        <template #cell(tax)="ctx">

                          <span class="small">{{ formatNumber(ctx.item.tax) }}</span>

                        </template>

  

                        <template #cell(sub_total)="ctx">

                          <b-form-input :value="formatNumber(ctx.item.sub_total)" readonly />

                        </template>

  

                        <!-- Row actions -->

                        <template #cell(actions)="ctx">

                          <div class="text-right">

                            <b-button size="sm" variant="outline-secondary" class="mr-1"

                              @click="duplicateRow(ctx.item)">

                              <i class="bi bi-files"></i>

                            </b-button>

                            <b-button size="sm" variant="danger" @click="removeRow(ctx.index)">

                              <i class="bi bi-trash"></i>

                            </b-button>

                          </div>

                        </template>

                      </b-table>

  

                      <!-- Totals -->

                      <b-card class="mt-3">

                        <b-row class="justify-content-end">

                          <b-col md="6" class="p-0">

                            <b-table-simple bordered striped small responsive>

                              <b-tbody>

                                <b-tr v-for="(row, i) in summaryRows" :key="i">

                                  <b-td class="font-weight-600">{{ row.label }}</b-td>

                                  <b-td class="text-right">

                                    <b-badge variant="success" pill class="font-weight-600">

                                      {{ currentUser.currency }} {{ row.value }}

                                    </b-badge>

                                  </b-td>

                                </b-tr>

                              </b-tbody>

                            </b-table-simple>

                          </b-col>

                        </b-row>

                      </b-card>

                    </b-card-body>

                  </b-tab>

  

                  <b-tab value="solutions">

                    <template #title>

                      <i class="bi bi-check2-square mr-1"></i>

                        Tasks

                      <span v-if="tasks.length" class="tab-badge">{{ tasks.length }}</span>

                    </template>

                    <b-card-body>

                      <div v-if="taskLoading" class="py-4 text-center">

                        <b-spinner small class="mr-2" /> Loading tasks…

                      </div>

  

                      <div v-else>

                        <div v-if="!project">

                          <b-alert show variant="light" class="d-flex align-items-center">

                            <div>No project tasks linked to this deal yet.</div>

                            <b-button size="sm" variant="primary" class="ml-2" @click="refreshTasks">

                              <i class="bi bi-arrow-clockwise"></i> Refresh

                            </b-button>

                          </b-alert>

                        </div>

  

                        <div v-else>

                          <div class="d-flex justify-content-between align-items-center mb-2">

                            <div>

                              <h6 class="mb-0">{{ project.name }}</h6>

                              <small class="text-muted">

                                Status:

                                <b-badge :variant="taskStatusVariant(project.status)">

                                  {{ project.status || 'open' }}

                                </b-badge>

                              </small>

                            </div>

                            <b-button size="sm" variant="outline-secondary" @click="refreshTasks">

                              <i class="bi bi-arrow-clockwise"></i> Refresh

                            </b-button>

                          </div>

  

                          <b-table :items="tasks" :fields="taskFields" small bordered responsive="md"

                            thead-class="custom-table-header" class="rounded overflow-hidden">

                            <template #cell(status)="ctx">

                              <b-badge :variant="taskStatusVariant(ctx.item.status)">

                                {{ ctx.item.status }}

                              </b-badge>

                            </template>

  

                            <template #cell(assignee)="ctx">

                              <span v-if="ctx.item.assignee_name && ctx.item.assignee_name">

                                {{ ctx.item.assignee_name }}

                              </span>

                              <span v-else-if="ctx.item.assignee_id">#{{ ctx.item.assignee_id }}</span>

                              <span v-else>—</span>

                            </template>

  

                            <template #cell(blocked_by)="ctx">

                              <span v-if="ctx.item.blocker_ids && ctx.item.blocker_ids.length">

                                {{ blockerNames(ctx.item) }}

                              </span>

                              <span v-else>—</span>

                            </template>

  

                            <template #cell(actions)="ctx">

                              <b-button v-if="(ctx.item.status || 'todo') !== 'done'" size="sm" variant="success"

                                @click="markTaskDone(ctx.item)">

                                <i class="bi bi-check2"></i>

                              </b-button>

                            </template>

  

                            <template #cell(updated_at)="ctx">

                              <span v-if="ctx.item.updated_at">

                                {{ formatDate(ctx.item.updated_at) }}

                              </span>

                              <span v-else>—</span>

                            </template>

                          </b-table>

                        </div>

                      </div>

                    </b-card-body>

                  </b-tab>

  
  

                  <b-tab value="activity" title="Activity" v-if="false">

                    <template #title><i class="bi bi-journal-text mr-1"></i> Activity</template>

                    <b-card-body>

                      <div class="mb-3">

                        <div class="small text-muted mb-2">Next Step</div>

                        <b-form-textarea v-model="form_data.next_step" rows="2" placeholder="E.g. Book site survey" />

                        <b-button block size="sm" variant="primary" class="mt-2" @click="saveNextStep">

                          Save Next Step

                        </b-button>

                      </div>

  

                      <!-- Timeline -->

                      <ul class="timeline">

                        <li v-for="(ev, idx) in activity" :key="idx">

                          <span class="time">{{ ev.date }}</span>

                          <span class="icon"><i :class="ev.icon || 'bi bi-chat-left-dots'"></i></span>

                          <div class="content" v-html="ev.content"></div>

                        </li>

                      </ul>

                    </b-card-body>

                  </b-tab>

  

                  <b-tab value="files">

                    <template #title><i class="bi bi-files mr-1"></i> Files</template>

  

                    <b-card-body>

                      <!-- your uploads / attachments -->

                    </b-card-body>

                  </b-tab>

  

                  <b-tab value="business_cases" >

                    <template #title><i class="bi bi-person-lines-fill mr-1"></i> Business Cases</template>

                  </b-tab>

                </b-tabs>

              </b-card>

            </b-col>

  

            <!-- RIGHT: Sticky summary -->

            <!-- <b-col md="3" class="mb-3 summary-col ml-auto" :class="{ collapsed: isSummaryCollapsed }"> -->

  

  <b-col v-if="!isSummaryCollapsed" md="3" class="mb-3 summary-col">

  <!-- TOP CARD: Amount / Owner / Actions -->

  <b-card class="mb-3 sticky-summary shadow-soft">

  <b-button

        type="button"

        class="btn btn-sm d-flex align-items-center"

        @click="isSummaryCollapsed = true"

      >

        <i class="bi bi-chevron-right mr-1"></i>

        Hide Summary

  </b-button>

    <div class="d-flex justify-content-between align-items-center mt-2">

      <div class="small text-muted">Amount (VAT incl.)</div>

      <b-badge variant="primary" pill class="font-weight-600">

        {{ currentUser.currency }} {{ dealAmount }}

      </b-badge>

    </div>

    <hr />

    <div class="d-flex justify-content-between">

      <div class="small text-muted">Expected Close</div>

      <div :class="['font-weight-600', isOverdue ? 'text-danger' : '']">

        {{ formatDate(form_data.expected_close_date) || '—' }}

      </div>

    </div>

    <div class="justify-content-between mt-2">

      <UserSelect

        class="user-select"

        v-model="form_data.responsible_person_id"

        :assignToMeId="currentUser && currentUser.id"

        :showAssignToMe="true"

        :showLabel="true"

        :rules="{ required: true }"

        :isOnlyEze24User="true"

      />

    </div>

    <b-button block variant="outline-danger" class="mt-3" @click="markLost">

      <i class="bi bi-x-octagon"></i> Mark Lost

    </b-button>

  </b-card>

  
  
  

</b-col>

  
  

 </b-row>

        </b-form>

      </validation-observer>

    </template>

  </PanelCreate>

</template>

  

<script>

import dealCreateMixin from "@/mixins/crm/deals/dealCreateMixin";

const DEAL_PIPELINE = {

  CONNECTIVITY: 0,

  NON_CONNECTIVITY: 1

}

  

const STATIC_PIPELINES = [

  { label: 'Connectivity', value: DEAL_PIPELINE.CONNECTIVITY },

  { label: 'Non-Connectivity', value: DEAL_PIPELINE.NON_CONNECTIVITY }

]

  

const STATIC_CONNECTIVITY_OPTIONS = [

  { label: 'GPON', value: 0, pipeline: DEAL_PIPELINE.CONNECTIVITY },

  { label: 'P2P', value: 1, pipeline: DEAL_PIPELINE.CONNECTIVITY },

  {

    label: 'No Pre-Sales Requirement',

    value: 2,

    pipeline: DEAL_PIPELINE.NON_CONNECTIVITY

  },

  {

    label: 'Pre-Sales Requirement',

    value: 3,

    pipeline: DEAL_PIPELINE.NON_CONNECTIVITY

  }

]

  

export default {

  mixins: [dealCreateMixin],

  

  metaInfo: {

    title: "CRM | Deal"

  },

  

    data() {

    return {

      pipelineIdProxy: null,

      form_data: {

        pipeline_id: null,

        connectivity_type: null

      },

  

      pipelines: STATIC_PIPELINES,

      connectivityOptionsRaw: STATIC_CONNECTIVITY_OPTIONS

    }

  },

  

   computed: {

    connectivityOptions() {

      if (this.pipelineIdProxy === null) return []

  

      return this.connectivityOptionsRaw.filter(

        o => o.pipeline === this.pipelineIdProxy

      )

    }

  },

  

    computed: {

    connectivityOptions() {

      console.log('Selected pipeline:', this.pipelineIdProxy)

  

      if (this.pipelineIdProxy === null) return []

  

      const filtered = this.connectivityOptionsRaw.filter(

        o => o.pipeline === this.pipelineIdProxy

      )

  

      console.log('Filtered connectivity options:', filtered)

  

      return filtered

    }

  },

  

  watch: {

    pipelineIdProxy(newVal) {

      console.log('Pipeline changed →', newVal)

  

      // sync to form_data

      this.form_data.pipeline_id = newVal

  

      // reset dependent field

      this.form_data.connectivity_type = null

    }

  },

  

//   computed: {

//   connectivityOptions() {

//     const pipelineType = PIPELINE_TYPE_MAP[this.pipelineIdProxy]

  

//     console.log('pipelineIdProxy:', this.pipelineIdProxy)

//     console.log('pipelineType:', pipelineType)

  

//     if (pipelineType === undefined) return []

  

//     return this.connectivityOptionsRaw.filter(

//       o => o.pipeline === pipelineType

//     )

//   }

// }

  
  

};

</script>

  

<style scoped>

.summary-col {

  animation: slideIn .25s ease-out;

}

  

@keyframes slideIn {

  from {

    opacity: 0;

    transform: translateX(16px);

  }

  to {

    opacity: 1;

    transform: translateX(0);

  }

}

  

.summary-fab {

  position: fixed;

  top: 50%;

  right: 0;

  transform: translateY(-50%);

  z-index: 1050;

  

  background: #fff;

  border: 1px solid #e5e7eb;

  border-right: none;

  border-radius: 10px 0 0 10px;

  

  padding: 8px 12px;

  display: flex;

  align-items: center;

  gap: 6px;

  

  box-shadow: -2px 4px 12px rgba(0,0,0,.08);

  cursor: pointer;

}

  

  .sticky-top {

  position: sticky;

  top: 60px; /* same as your previous margin-top */

}

  
  

  .summary-expand-btn {

    position: fixed;

    top: 50%;

    right: 0;

    transform: translateY(-50%);

    z-index: 1050;

  

    background: #ffffff;

    border: 1px solid #e5e7eb;

    border-right: none;

    border-radius: 8px 0 0 8px;

  

    padding: 8px 12px;

    box-shadow: -2px 4px 12px rgba(0, 0, 0, 0.08);

  

    color: #4b5563;

    cursor: pointer;

    white-space: nowrap;

  }

  

  .summary-expand-btn:hover {

    background: #f9fafb;

    color: #111827;

  }

  

  .summary-expand-label {

    font-size: 12px;

    font-weight: 500;

  }

  .summary-expand-btn:hover {

    background: #f9fafb;

    color: #111827;

  }

  

  .summary-expand-btn i {

    font-size: 18px;

  }

  

  .transition-col {

    transition: width 0.3s ease;  

    /* transition: margin 0.25s ease, width 0.25s ease; */

  

  }

  

  .summary-col {

    transition: all 0.3s ease;

    /* overflow: hidden; */

  }

  

  .sticky-card {

    position: sticky;

    top: 80px;

  }

  

/* Sticky summary */

.sticky-summary {

  position: sticky;

  top: 160px;

  z-index: 10;

}

  

  .summary-toggle {

    color: #6b7280;

  }

  

  .summary-toggle:hover {

    color: #111827;

    text-decoration: none;

  }

  

  .card-header-line {

    user-select: none;

  }

  

/* Labels */

.form-group label {

  font-size: 14px;

  color: #4B5563;

  margin-bottom: 4px;

}

  

label {

  font-size: 12px;

  color: #4B5563;

  margin-bottom: 4px;

}

  

/** UOM Start */

.uom-select {

  min-width: 120px !important;

}

  

.uom-select .vs__dropdown-toggle {

  height: 32px;            /* match .form-control-sm */

  padding: 0 6px;

  border-radius: 0.2rem;

}

  

.uom-select .vs__selected {

  margin: 0;

  font-size: 0.875rem;

  line-height: 30px;

}

  

.uom-select .vs__actions {

  padding-top: 0;

}

  

.uom-select .vs__dropdown-toggle {

  display: flex;

  align-items: center;

}

.uom-select .vs__dropdown-menu {

  z-index: 9999999 !important;

}

:deep(.uom-select .vs__dropdown-toggle) {

  height: 32px;

}

  

/* Make vue-select look like Bootstrap input-sm */

.table-input.v-select {

  font-size: 0.875rem;

}

  

.table-input .vs__dropdown-toggle {

  height: 34px;                 /* same as form-control */

  padding: 0 8px;

  border: 1px solid #ced4da;

  border-radius: 4px;

  background-color: #fff;

  display: flex;

  align-items: center;

}

  

.table-input .vs__selected {

  margin: 0;

  padding: 0;

  line-height: 32px;

}

  

.table-input .vs__actions {

  padding: 0;

}

  

.table-input .vs__open-indicator {

  transform: scale(0.8);

}

  

/* Dropdown visibility */

.table-input .vs__dropdown-menu {

  z-index: 99999999 !important;

}

  

/* Remove extra spacing inside table */

table td .table-input {

  margin-bottom: 0;

}

  
  

/** UOM */

  
  

/* Card polish */

.hero-card {

  border: 1px solid #e5e7eb;

  border-radius: .75rem;

}

  

.gap-8>* {

  margin-right: .5rem;

}

  

/* =========================

   Deal Stepper

   ========================= */

/* Deal Stepper — equal width on desktop, 100% per step + scroll on small screens */

.deal-stepper {

  display: grid;

  grid-auto-flow: column;

  grid-auto-columns: 1fr;

  /* desktop: each step shares the row */

  gap: .75rem;

  width: 100%;

  overflow-x: auto;

  padding: .25rem 0;

  -webkit-overflow-scrolling: touch;

  scrollbar-width: thin;

  scroll-snap-type: x proximity;

  /* smooth snap */

}

  

.deal-stepper::-webkit-scrollbar {

  height: 6px;

}

  

.deal-stepper::-webkit-scrollbar-thumb {

  background: #e5e7eb;

  border-radius: 999px;

}

  

@media (max-width: 992px) {

  .deal-stepper {

    grid-auto-columns: 100%;

    /* mobile: each step fills the card */

    scroll-snap-type: x mandatory;

  }

}

  

.deal-stepper .step {

  display: flex;

  align-items: center;

  gap: .5rem;

  width: 100%;

  padding: .4rem .9rem;

  border: 1px solid #e5e7eb;

  border-radius: 999px;

  background: #f8fafc;

  white-space: nowrap;

  cursor: pointer;

  user-select: none;

  transition: transform .06s ease-in-out, box-shadow .12s ease-in-out, border-color .12s;

  scroll-snap-align: start;

}

  

.user-select {

  padding: 0px;

}

  

.deal-stepper .step:hover {

  transform: translateY(-1px);

}

  

.deal-stepper .step .index {

  width: 22px;

  height: 22px;

  line-height: 22px;

  text-align: center;

  border-radius: 999px;

  border: 1px solid #e5e7eb;

  font-size: 12px;

  background: #fff;

}

  

.deal-stepper .step .label {

  font-size: 13px;

  display: inline-flex;

  align-items: center;

  gap: .375rem;

}

  

.deal-stepper .step .label .nav-icon {

  /* icon spacing */

  font-size: 14px;

}

  

/* Active / Done states */

.deal-stepper .step.active {

  background: #eef2ff;

  /* indigo-100 */

  border-color: #c7d2fe;

  /* indigo-200 */

  box-shadow: 0 1px 0 rgba(0, 0, 0, .03);

}

  

.deal-stepper .step.active .index {

  background: #4f46e5;

  color: #fff;

  border-color: #4f46e5;

}

  

.deal-stepper .step.done {

  background: #f0fdf4;

  /* green-50 */

  border-color: #bbf7d0;

  /* green-200 */

}

  

.deal-stepper .step.done .index {

  background: #16a34a;

  color: #fff;

  border-color: #16a34a;

}

  

/* Stage color accents (use slug names from label -> lower-case + hyphens) */

.deal-stepper .stage-lead.active {

  background: #f3f4f6;

  border-color: #d1d5db;

}

  

/* secondary */

.deal-stepper .stage-qualify.active {

  background: #e0f2fe;

  border-color: #bae6fd;

}

  

/* info */

.deal-stepper .stage-site-survey.active {

  background: #eef2ff;

  border-color: #c7d2fe;

}

  

/* primary */

.deal-stepper .stage-proposal.active {

  background: #fff7ed;

  border-color: #fde68a;

}

  

/* warning */

.deal-stepper .stage-negotiation.active {

  background: #ede9fe;

  border-color: #ddd6fe;

}

  

/* dark-ish accent */

.deal-stepper .stage-close.active {

  background: #f3f4f6;

  border-color: #d1d5db;

}

  

/* secondary */

.deal-stepper .stage-close-deal.active {

  background: #dcfce7;

  border-color: #bbf7d0;

}

  

/* success */

  

/* Keyboard focus */

.deal-stepper .step:focus-visible {

  outline: 2px solid #93c5fd;

  outline-offset: 2px;

}

  

/* Responsive: tighten pills a bit on small screens */

@media (max-width: 576px) {

  .deal-stepper .step {

    padding: .35rem .6rem;

  }

  

  .deal-stepper .step .index {

    width: 20px;

    height: 20px;

    line-height: 20px;

    font-size: 11px;

  }

  

  .deal-stepper .step .label {

    font-size: 12px;

  }

}

  
  

/* Table header */

:deep(.custom-table-header) {

  background-color: #F3F4F6;

  font-weight: 600;

}

  

/* Timeline */

.timeline {

  list-style: none;

  padding: 0;

  margin: 0;

}

  

.timeline li {

  display: flex;

  align-items: flex-start;

  margin-bottom: 1rem;

}

  

.timeline .time {

  width: 100px;

  font-size: 12px;

  color: #6b7280;

}

  

.timeline .icon {

  width: 24px;

  text-align: center;

  color: #4f46e5;

  margin-right: .5rem;

}

  

.timeline .content {

  background: #f9fafb;

  padding: .5rem .75rem;

  border-radius: .5rem;

  flex: 1;

}

  

/* Badges */

.badge {

  font-size: 12px;

}

  

/* Tabs body padding reset (your request) */

.tab-pane.card-body {

  padding: 0 !important;

}

  

/* Make containers inside this page full width */

.deal-page :deep(.container),

.deal-page :deep(.container-sm),

.deal-page :deep(.container-md),

.deal-page :deep(.container-lg),

.deal-page :deep(.container-xl),

.deal-page :deep(.container-xxl) {

  max-width: 100% !important;

  padding-left: 0;

  padding-right: 0;

}

  

/* Ensure cards stretch fully */

.deal-page :deep(.card),

.deal-page :deep(.card-body) {

  width: 100%;

}

  

/* Stepper can use full row on large screens and still scroll on small ones */

.deal-stepper {

  flex-wrap: wrap;

  /* allow using full width if space available */

  justify-content: flex-start;

}

  

@media (max-width: 768px) {

  .deal-stepper {

    flex-wrap: nowrap;

    overflow-x: auto;

  }

}

  

::v-deep .nav {

  margin-top: 0px !important;

}

  

::v-deep ::placeholder {

  font-size: 12px;

  color: #6b7280;

}

  

/* Numeric cells: don’t wrap; inputs have room and are right-aligned */

.col-num {

  white-space: nowrap;

}

  

.num-input {

  text-align: right;

  min-width: 6.5rem;

}

  

/* ~104px */

  

/* Product needs more room so it doesn't crush others */

.col-prod {

  min-width: 260px;

}

  

/* Actions column stays compact */

.col-actions {

  width: 80px;

}

  

/* On medium screens, keep columns readable */

@media (max-width: 992px) {

  .col-tight {

    width: 9.5rem;

  }

  

  /* give qty/discount% a fixed-ish width */

}

  

/* Optional: if you want even more space on ~tablet, hide discount_amount */

@media (max-width: 992px) {

  /* .col-discount_amount { display: none; }   uncomment if needed */

}

  

/* keep % beside the input, never below */

.num-group.input-group {

  flex-wrap: nowrap;

}

  

/* numeric input: allow shrink but keep some room */

.num-group .form-control {

  flex: 1 1 0;

  min-width: 6.5rem;

  /* tweak if needed */

  text-align: right;

}

  

/* addon stays compact */

.num-group .input-group-append {

  flex: 0 0 auto;

}

  

.num-group .input-group-text {

  white-space: nowrap;

  padding-left: .5rem;

  padding-right: .5rem;

}

  

/* ensure the Discount (%) column can't collapse too far */

.col-discount-percent {

  min-width: 8.5rem;

}

  
  

/* Modal shell */

.modern-modal {

  border-radius: 16px;

  overflow: hidden;

}

  

.modern-modal-dialog {

  max-width: 980px;

}

  

/* Header */

.mm-header {

  display: flex;

  align-items: center;

  justify-content: space-between;

  padding: 14px 18px;

  background: linear-gradient(90deg, #eef2ff 0%, #f8fafc 100%);

  border-bottom: 1px solid #e5e7eb;

}

  

.mm-h-left {

  display: flex;

  align-items: center;

  gap: 12px;

}

  

.mm-avatar {

  width: 42px;

  height: 42px;

  border-radius: 50%;

  display: grid;

  place-items: center;

  background: #4f46e5;

  color: #fff;

  font-size: 18px;

  box-shadow: 0 2px 6px rgba(79, 70, 229, .25);

}

  

.mm-title {

  font-weight: 700;

  font-size: 16px;

  color: #111827;

  line-height: 1.15;

}

  

.mm-sub {

  font-size: 12px;

  color: #6b7280;

}

  

.mm-close {

  color: #374151;

}

  

.mm-close:hover {

  color: #111827;

}

  

/* Chips */

.mm-chips {

  display: flex;

  gap: 8px;

  padding: 8px 16px 10px;

  background: #f8fafc;

  border-bottom: 1px solid #e5e7eb;

}

  

.mm-chip {

  display: inline-flex;

  align-items: center;

  gap: 6px;

  padding: 4px 10px;

  font-size: 12px;

  border: 1px solid #e5e7eb;

  border-radius: 999px;

  background: #ffffff;

}

  

.mm-chip.active {

  background: #eef2ff;

  border-color: #c7d2fe;

}

  

/* Body */

.mm-body {

  padding: 14px 16px 18px;

  background: #ffffff;

}

  

/* Sections */

.mm-section {

  border: 1px solid #e5e7eb;

  border-radius: 12px;

  padding: 14px;

  margin-bottom: 14px;

  box-shadow: 0 1px 0 rgba(0, 0, 0, .03);

}

  

.mm-section-head {

  display: flex;

  align-items: center;

  gap: 10px;

  margin-bottom: 10px;

}

  

.mm-section-icon {

  width: 28px;

  height: 28px;

  border-radius: 8px;

  display: grid;

  place-items: center;

  background: #eef2ff;

  color: #4f46e5;

}

  

.mm-section-icon.info {

  background: #e0f2fe;

  color: #0284c7;

}

  

.mm-section-icon.success {

  background: #dcfce7;

  color: #16a34a;

}

  

.mm-section-title {

  font-weight: 600;

  color: #111827;

}

  

.mm-section-sub {

  font-size: 12px;

  color: #6b7280;

}

  

/* Labels within grouped inputs */

.mm-label {

  display: block;

  font-size: 12px;

  color: #4B5563;

  margin-bottom: 4px;

}

  

/* Input group polish */

:deep(.input-group-text) {

  background: #f3f4f6;

  border-color: #e5e7eb;

}

  

:deep(.input-group .form-control) {

  border-left: 0;

}

  

/* ---------- Sizing the dialog (desktop) ---------- */

.mm-dialog-wide {

  /* BootstrapVue applies this class to .modal-dialog; we set a custom width */

  width: 96vw;

  /* go almost full width */

  max-width: 1240px;

  /* cap it on large screens */

  margin: 10px auto;

  /* nice breathing room */

}

  

@media (min-width: 1600px) {

  .mm-dialog-wide {

    max-width: 1400px;

  }

}

  

/* Optional: taller viewport on large screens */

:deep(.modal-content.deal-modal) {

  border-radius: 16px;

}

  

:deep(.deal-modal .modal-body.mm-body) {

  max-height: calc(100vh - 140px);

  /* header/footer minus some gap */

  overflow: auto;

}

  

/* ---------- Section cards polish (kept from earlier) ---------- */

.modern-modal {

  border-radius: 16px;

  overflow: hidden;

}

  

.mm-body {

  background: #ffffff;

}

  

/* Keep your section shells tight */

.mm-section {

  border: 1px solid #e5e7eb;

  border-radius: 12px;

  padding: 14px;

  margin: 12px 16px;

  box-shadow: 0 1px 0 rgba(0, 0, 0, .03);

}

  

.mm-section-head {

  display: flex;

  align-items: center;

  gap: 10px;

  margin-bottom: 10px;

}

  

.mm-section-icon {

  width: 28px;

  height: 28px;

  border-radius: 8px;

  display: grid;

  place-items: center;

  background: #eef2ff;

  color: #4f46e5;

}

  

.mm-section-icon.info {

  background: #e0f2fe;

  color: #0284c7;

}

  

.mm-section-icon.success {

  background: #dcfce7;

  color: #16a34a;

}

  

.mm-section-title {

  font-weight: 600;

  color: #111827;

}

  

.mm-section-sub {

  font-size: 12px;

  color: #6b7280;

}

  

/* ---------- INPUT NORMALIZATION (works for <FormInput> & <FormSelect>) ---------- */

/* Base input height + radius for Bootstrap inputs used inside your components */

:deep(.deal-modal .form-control) {

  min-height: 40px;

  height: 40px;

  /* ensures consistent boxes */

  border-radius: 10px;

  border-color: #e5e7eb;

}

  

/* vue-select (used by your FormSelect) wrapper */

:deep(.deal-modal .v-select .vs__dropdown-toggle) {

  min-height: 40px;

  border-radius: 10px;

  border-color: #e5e7eb;

  padding-top: 2px;

  /* aligns text vertically */

  padding-bottom: 2px;

}

  

/* vue-select text and search alignment */

:deep(.deal-modal .v-select .vs__selected),

:deep(.deal-modal .v-select .vs__search) {

  margin-top: 3px;

  /* vertically center text inside 40px */

}

  

/* Input placeholders & labels look calmer */

:deep(.deal-modal ::placeholder) {

  color: #9ca3af;

  font-size: 12px;

}

  

:deep(.deal-modal label) {

  font-size: 12px;

  color: #4b5563;

  margin-bottom: 4px;

}

  

/* Reduce default group margins a bit for dense layout */

:deep(.deal-modal .form-group) {

  margin-bottom: 10px;

}

  

/* Fix nested input-group outlines if any */

:deep(.deal-modal .input-group .form-control) {

  border-left: 0;

}

  

:deep(.deal-modal .input-group-text) {

  background: #f3f4f6;

  border-color: #e5e7eb;

  border-radius: 10px 0 0 10px;

}

  

/* Row gutters a bit tighter so fields line up nicely */

:deep(.deal-modal .row) {

  margin-left: -6px;

  margin-right: -6px;

}

  

:deep(.deal-modal .col),

:deep(.deal-modal [class*="col-"]) {

  padding-left: 6px;

  padding-right: 6px;

}

  

/* Buttons in footer a bit chunkier */

:deep(.modal-footer .btn) {

  border-radius: 10px;

}

  

/* ---------- Header chips (if you kept them) ---------- */

.mm-chips {

  display: flex;

  gap: 8px;

  padding: 8px 16px 10px;

  background: #f8fafc;

  border-bottom: 1px solid #e5e7eb;

}

  

.mm-chip {

  display: inline-flex;

  align-items: center;

  gap: 6px;

  padding: 4px 10px;

  font-size: 12px;

  border: 1px solid #e5e7eb;

  border-radius: 999px;

  background: #fff;

}

  

.mm-chip.active {

  background: #eef2ff;

  border-color: #c7d2fe;

}

  

/* ---------- Mobile tweaks ---------- */

@media (max-width: 768px) {

  .mm-dialog-wide {

    width: 98vw;

    max-width: 98vw;

  }

  

  .mm-section {

    margin: 10px;

    padding: 12px;

  }

  

  :deep(.deal-modal .form-control),

  :deep(.deal-modal .v-select .vs__dropdown-toggle) {

    min-height: 38px;

    height: 38px;

  }

}

  

.mm-dialog-wide {

  width: 99vw;

  max-width: 99vw;

}

  

/* ---------- Dialog sizing ---------- */

.mm-dialog {

  margin: 10px auto;

}

  

.mm-dialog-wide {

  width: 96vw;

  max-width: 1240px;

}

  

.mm-dialog-full {

  width: 99vw;

  max-width: 1600px;

}

  

@media (min-width: 1680px) {

  .mm-dialog-full {

    max-width: 1760px;

  }

}

  

:deep(.modal-content.deal-modal) {

  border-radius: 16px;

  overflow: hidden;

}

  

:deep(.deal-modal .modal-body.mm-body) {

  max-height: calc(100vh - 140px);

  overflow: auto;

}

  

/* ---------- Header ---------- */

.mm-header {

  display: flex;

  align-items: center;

  justify-content: space-between;

  padding: 14px 18px;

  background: linear-gradient(90deg, #eef2ff 0%, #f8fafc 100%);

  border-bottom: 1px solid #e5e7eb;

}

  

.mm-h-left {

  display: flex;

  align-items: center;

  gap: 12px;

}

  

.mm-h-actions {

  display: flex;

  align-items: center;

}

  

.mm-avatar {

  width: 42px;

  height: 42px;

  border-radius: 50%;

  display: grid;

  place-items: center;

  background: #4f46e5;

  color: #fff;

  font-size: 18px;

  box-shadow: 0 2px 6px rgba(79, 70, 229, .25);

}

  

.mm-title {

  font-weight: 700;

  font-size: 16px;

  color: #111827;

}

  

.mm-sub {

  font-size: 12px;

  color: #6b7280;

}

  

.mm-close {

  color: #374151;

}

  

.mm-close:hover {

  color: #111827;

}

  

.mm-chips {

  display: flex;

  gap: 8px;

  padding: 8px 16px 10px;

  background: #f8fafc;

  border-bottom: 1px solid #e5e7eb;

}

  

.mm-chip {

  display: inline-flex;

  align-items: center;

  gap: 6px;

  padding: 4px 10px;

  font-size: 12px;

  border: 1px solid #e5e7eb;

  border-radius: 999px;

  background: #fff;

}

  

.mm-chip.active {

  background: #eef2ff;

  border-color: #c7d2fe;

}

  

/* ---------- Sections ---------- */

.mm-section {

  width: 100%;

  border: 1px solid #e5e7eb;

  border-radius: 12px;

  padding: 14px;

  margin: 12px 16px;

  box-shadow: 0 1px 0 rgba(0, 0, 0, .03);

  background: #fff;

}

  

.mm-section-head {

  display: flex;

  align-items: center;

  gap: 10px;

  margin-bottom: 10px;

}

  

.mm-section-icon {

  width: 28px;

  height: 28px;

  border-radius: 8px;

  display: grid;

  place-items: center;

  background: #eef2ff;

  color: #4f46e5;

}

  

.mm-section-icon.info {

  background: #e0f2fe;

  color: #0284c7;

}

  

.mm-section-icon.success {

  background: #dcfce7;

  color: #16a34a;

}

  

.mm-section-title {

  font-weight: 600;

  color: #111827;

}

  

.mm-section-sub {

  font-size: 12px;

  color: #6b7280;

}

  

/* ---------- Tighten grid gutters ---------- */

:deep(.deal-modal .row) {

  margin-left: -6px;

  margin-right: -6px;

}

  

:deep(.deal-modal .col),

:deep(.deal-modal [class*="col-"]) {

  padding-left: 6px;

  padding-right: 6px;

}

  

/* ---------- Input normalization ---------- */

:deep(.deal-modal label) {

  font-size: 12px;

  color: #4b5563;

  margin-bottom: 4px;

  white-space: nowrap;

  text-overflow: ellipsis;

  overflow: hidden;

}

  

:deep(.deal-modal .form-group) {

  margin-bottom: 10px;

}

  

/* Native inputs */

:deep(.deal-modal .form-control) {

  height: 44px;

  min-height: 44px;

  border-radius: 10px;

  border-color: #e5e7eb;

}

  

/* Input groups with icons */

:deep(.deal-modal .input-group-text) {

  background: #f3f4f6;

  border-color: #e5e7eb;

  border-radius: 10px 0 0 10px;

}

  

:deep(.deal-modal .input-group .form-control) {

  border-left: 0;

}

  

/* vue-select inside your FormSelect */

:deep(.deal-modal .v-select .vs__dropdown-toggle) {

  min-height: 44px;

  border-radius: 10px;

  border-color: #e5e7eb;

  padding-top: 2px;

  padding-bottom: 2px;

}

  

:deep(.deal-modal .v-select .vs__selected-options) {

  padding-top: 2px;

}

  

:deep(.deal-modal .v-select .vs__selected),

:deep(.deal-modal .v-select .vs__search) {

  margin-top: 4px;

}

  

/* Placeholders */

:deep(.deal-modal ::placeholder) {

  color: #9ca3af;

  font-size: 12px;

}

  

/* Footer buttons */

:deep(.modal-footer .btn) {

  border-radius: 10px;

}

  

/* ---------- Mobile ---------- */

@media (max-width: 768px) {

  

  .mm-dialog-wide,

  .mm-dialog-full {

    width: 98vw;

    max-width: 98vw;

  }

  

  .mm-section {

    margin: 10px;

    padding: 12px;

  }

  

  :deep(.deal-modal .form-control) {

    height: 40px;

    min-height: 40px;

  }

  

  :deep(.deal-modal .v-select .vs__dropdown-toggle) {

    min-height: 40px;

  }

}

  

.flat-card {

  box-shadow: none !important;

  border: 1px solid #c7c7c7 !important;

}

  
  

/* --- Fix: keep Pipeline select from collapsing --- */

.hero-meta {

  gap: 12px;

  flex-wrap: nowrap;                 /* row by default */

}

@media (max-width: 576px) {

  .hero-meta { flex-wrap: wrap; }    /* stack on phones */

}

  

/* Give the pipeline area a real flex-basis, not auto/0 */

.hero-meta__pipeline {

  flex: 1 1 320px;                   /* grows, but never below ~320px */

  min-width: 260px;                  /* protect on narrow screens */

  max-width: 460px;                  /* tidy next to dates */

}

  

/* Make inner wrappers stretch */

.hero-meta__pipeline :deep(.form-group),

.hero-meta__pipeline :deep(.v-select),

.hero-meta__pipeline :deep(.vs__dropdown-toggle) {

  width: 100%;

  min-width: 0;                      /* prevent flex overflow → 0px bug */

}

  

/* If FormSelect injects Bootstrap cols, neutralize them */

.hero-meta__pipeline :deep([class*="col-"]) {

  flex: 1 1 100%;

  max-width: 100%;

}

  

/* vue-select internals: prevent squish */

.hero-meta__pipeline :deep(.vs__selected-options),

.hero-meta__pipeline :deep(.vs__actions) {

  min-width: 0;                      /* allow content to shrink properly */

}

  

/* Validation message: keep it neat */

.hero-meta__pipeline :deep(.invalid-feedback) {

  margin-top: 4px;

  white-space: normal;               /* wrap instead of vertical clipping */

  word-break: break-word;

}

  

/* Dates block stays compact and near the select */

.hero-meta__dates {

  flex: 0 1 240px;

}

@media (min-width: 768px) {

  .hero-meta__dates {

    padding-left: 14px;

    border-left: 1px solid rgba(0,0,0,0.08);

  }

}

@media (max-width: 575.98px) {

  .hero-meta__dates {

    text-align: left;

    border-left: 0;

    padding-left: 0;

  }

}

  

.tite-pill {

    display: grid;

    place-items: center;

    width: 36px;

    height: 36px;

    border-radius: 999px;

    background: #eef2ff;

    color: #4338ca;

    box-shadow: inset 0 1px 0 rgba(255, 255, 255, .85);

}

  

.avatar-24 { width:24px; height:24px; border-radius:50%; object-fit:cover; }

.avatar-20 { width:20px; height:20px; border-radius:50%; object-fit:cover; }

  

/* compact height to match your sticky card vibe */

.sticky-summary :deep(.owner-vselect .vs__dropdown-toggle) { min-height: 36px; }

.sticky-summary :deep(.owner-vselect .vs__selected) { margin-top: 2px; }

  
  

/* Tabs Header Style*/

/* --- Pretty tabs (pills + underline indicator) --- */

.deal-page :deep(.card-header) {

  padding: 10px 12px;

  background: #fff;

  border-bottom: 1px solid #e5e7eb;

}

  

.deal-page :deep(.qtabs) {

  display: flex;

  gap: 8px;

  /* modern spacing */

  overflow-x: auto;

  /* nice on mobile */

  scrollbar-width: thin;

}

  

.deal-page :deep(.qtabs .nav-item) {

  margin: 0;

}

  

.deal-page :deep(.qtabs .nav-link) {

  border: 0 !important;

  /* remove bootstrap border */

  background: transparent;

  color: #6b7280;

  /* slate-500 */

  font-weight: 600;

  padding: 8px 12px;

  border-radius: 12px;

  /* pill */

  position: relative;

  transition: all .18s ease;

  white-space: nowrap;

}

  

.deal-page :deep(.qtabs .nav-link:hover) {

  color: #111827;

  /* slate-900 */

  background: #f8fafc;

  /* light hover */

  box-shadow: inset 0 0 0 1px #e5e7eb;

}

  

/* Active pill */

.deal-page :deep(.qtabs .nav-link.active) {

  color: #111827;

  background: #eef2ff;

  /* subtle indigo bg */

  box-shadow: inset 0 0 0 1px #dbeafe;

}

  

/* Underline indicator for active pill */

.deal-page :deep(.qtabs .nav-link.active::after) {

  content: "";

  position: absolute;

  left: 12px;

  right: 12px;

  bottom: -10px;

  height: 3px;

  border-radius: 999px;

  background: #f59e0b;

  /* amber brand line */

}

  

/* Make icons align nicely */

.deal-page :deep(.qtabs .nav-link i) {

  vertical-align: -1px;

}

  

/* prettier pills */

.deal-page :deep(.qtabs) {

  display: flex;

  gap: 8px;

  overflow-x: auto;

}

  

.deal-page :deep(.qtabs .nav-link) {

  border: 0 !important;

  background: transparent;

  color: #6b7280;

  font-weight: 600;

  padding: 8px 12px;

  border-radius: 12px;

  transition: all .18s;

}

  

.deal-page :deep(.qtabs .nav-link:hover) {

  color: #111827;

  background: #f8fafc;

  box-shadow: inset 0 0 0 1px #e5e7eb;

}

  

.deal-page :deep(.qtabs .nav-link.active) {

  color: #111827;

  background: #fff7ed;

  /* soft amber */

  box-shadow: inset 0 0 0 1px #fed7aa;

}

  

.deal-page :deep(.qtabs .nav-link.active::after) {

  content: "";

  position: absolute;

  left: 12px;

  right: 12px;

  bottom: -10px;

  height: 3px;

  border-radius: 999px;

  background: #f59e0b;

}

  

.deal-page :deep(.qtabs .nav-link i) {

  vertical-align: -1px;

  color: #f59e0b;

}

  

/* inside <style scoped> */

.tab-badge {

  display: inline-block;

  margin-left: 6px;

  padding: 2px 6px;

  border-radius: 999px;

  font-size: 11px;

  font-weight: 700;

  line-height: 1;

  background: #fee2e2;

  color: #b91c1c;

}

  

.deal-page :deep(.tab-dot) {

  display: inline-block;

  width: 8px;

  height: 8px;

  margin-left: 6px;

  border-radius: 999px;

  background: #ef4444;

}

</style>

<style>

/* Only affects the Add Company modal */

.deal-add-company-modal .modal-dialog.mm-dialog-wide {

  width: 96vw;

  max-width: 1240px;

  margin: 10px auto;

}

  

.deal-add-company-modal .modal-dialog.mm-dialog-full {

  width: 99vw;

  max-width: 1600px;

  margin: 10px auto;

}

  

@media (min-width:1680px) {

  .deal-add-company-modal .modal-dialog.mm-dialog-full {

    max-width: 1760px;

  }

}

  

.deal-add-company-modal .modal-content.deal-modal {

  border-radius: 16px;

  overflow: hidden;

}

  

.deal-add-company-modal .modal-body.mm-body {

  max-height: calc(100vh - 140px);

  overflow: auto;

}

  

/* spacing + sections (optional) */

.deal-add-company-modal .mm-section {

  border: 1px solid #e5e7eb;

  border-radius: 12px;

  padding: 14px;

  margin: 12px 16px;

  box-shadow: 0 1px 0 rgba(0, 0, 0, .03);

  background: #fff;

}

  

/* input normalization (native + vue-select) */

.deal-add-company-modal .form-group {

  margin-bottom: 10px;

}

  

.deal-add-company-modal label {

  font-size: 12px;

  color: #4b5563;

  margin-bottom: 4px;

  white-space: nowrap;

  overflow: hidden;

  text-overflow: ellipsis;

}

  

.deal-add-company-modal .form-control {

  height: 44px;

  min-height: 44px;

  border-radius: 10px;

  border-color: #e5e7eb;

}

  

.deal-add-company-modal .input-group-text {

  background: #f3f4f6;

  border-color: #e5e7eb;

  border-radius: 10px 0 0 10px;

}

  

.deal-add-company-modal .input-group .form-control {

  border-left: 0;

}

  

.deal-add-company-modal .v-select .vs__dropdown-toggle {

  min-height: 44px;

  border-radius: 10px;

  border-color: #e5e7eb;

  padding-top: 2px;

  padding-bottom: 2px;

}

  

.deal-add-company-modal .v-select .vs__selected-options {

  padding-top: 2px;

}

  

.deal-add-company-modal .v-select .vs__selected,

.deal-add-company-modal .v-select .vs__search {

  margin-top: 4px;

}

  

.deal-add-company-modal .row {

  margin-left: -6px;

  margin-right: -6px;

}

  

.deal-add-company-modal .col,

.deal-add-company-modal [class*="col-"] {

  padding-left: 6px;

  padding-right: 6px;

}

  

@media (max-width:768px) {

  

  .deal-add-company-modal .modal-dialog.mm-dialog-wide,

  .deal-add-company-modal .modal-dialog.mm-dialog-full {

    width: 98vw;

    max-width: 98vw;

  }

  

  .deal-add-company-modal .form-control {

    height: 40px;

    min-height: 40px;

  }

  

  .deal-add-company-modal .v-select .vs__dropdown-toggle {

    min-height: 40px;

  }

  

  .card {

    border: 1px solid #e5e7eb;

    border-radius: 14px;

    overflow: hidden;

    box-shadow: 0 6px 18px rgba(0, 0, 0, .04);

  }

  

}

</style>
```