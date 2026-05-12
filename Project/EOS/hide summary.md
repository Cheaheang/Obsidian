button
```vue
  <button

  v-if="isBusinessCase"

  class="summary-fab"

  @click="isBusinessCase = false"

>

  <i class="bi bi-chevron-left"></i>

  <span>Quotation Summary</span>

</button>
```

body
```vue
<b-col v-if="!isBusinessCase" md="3" class="mb-3 summary-col">

  <b-card class="mb-3 sticky-summary shadow-soft">

  <b-button

        type="button"

        class="btn btn-sm d-flex align-items-center"

        @click="isBusinessCase = true"

      >

        <i class="bi bi-chevron-right mr-1"></i>

        Hide Business Case Summary

  </b-button>

  <div class="sticky-summary">

              </div>

  </b-card>

</b-col>
```

<b-col v-if="!isBusinessCase" md="3" class="mb-3 summary-col">

  <b-card class="mb-3 sticky-summary shadow-soft">

  <b-button

        type="button"

        class="btn btn-sm d-flex align-items-center"

        @click="isBusinessCase = true"

      >

        <i class="bi bi-chevron-right mr-1"></i>

        Hide Business Case Summary

  </b-button>

  <div class="sticky-summary">

              </div>

  </b-card>

</b-col>


















```vue
 <b-row>
   <b-col v-if="!isBusinessCase" md="3" class="mb-3 summary-col">

  <b-card class="mb-3 sticky-summary shadow-soft">
  <b-button
        type="button"
        class="btn btn-sm d-flex align-items-center"
        @click="isBusinessCase = true"
      >

        <i class="bi bi-chevron-right mr-1"></i>
        Hide Business Case Summary
  </b-button>
  <BusinessCaseSummary v-bind="summary" />
 </b-card>
</b-col>
  <b-col v-if="!isSummaryCollapsed" md="3" class="mb-3 summary-col">


  <b-card class="mb-3 sticky-summary shadow-soft">

  <b-button

        type="button"

        class="btn btn-sm d-flex align-items-center"

        @click="isSummaryCollapsed = true"

      >

        <i class="bi bi-chevron-right mr-1"></i>

        Hide Summary

  </b-button>
    <hr />

   <!-- RIGHT: Sticky summary (light) -->

            <b-col md="4" class="mb-3">

              <div class="sticky-summary">

                <b-card class="mb-3">

                  <div class="d-flex justify-content-between align-items-center">

                    <div class="small text-muted">Grand Total</div>

                    <b-badge variant="primary" pill class="font-weight-600">

                      {{ currentUser.currency }} {{ grandTotalDisplay }}

                    </b-badge>

                  </div>

                  <hr />

                  <div class="d-flex justify-content-between">

                    <div class="small text-muted">Valid until</div>

                    <div :class="['font-weight-600', isExpired ? 'text-danger' : '']">{{

                      formatDate(form_quote.valid_until) || '—'

                      }}</div>

                  </div>

                  <div class="justify-content-between mt-2">

                    <UserSelect class="user-select" v-model="form_quote.responsible_person_id" :assignToMeId="currentUser && currentUser.id"

                      :showAssignToMe="true" :showLabel="true" />

                  </div>

                  <b-button block variant="outline-danger" class="mt-2" @click="markRejected" v-show="false">

                    <i class="bi bi-x-octagon"></i> Mark Rejected

                  </b-button>

                </b-card>

              </div>

            </b-col>

  </b-card>

  
  
  

</b-col>

  
  

 </b-row>
```