<script setup>
import { reactive, ref } from 'vue'

// toggle button
const value = ref('')
const discount = ref(0)
const GstPlus = ref(0)
const TotalAmount = ref(0)


const ItemList = reactive({

  itemName: '',
  Qlt: 0,
  Uom: '',
  Rate: 0,
  Gst: 0,
  Discount: 0,
  Amount: 0
})

const InvoiceDetails = reactive({
  InvoiceNumber: 0,
  referenceNumber: 0,
  // Date: "dd-mm-yyyy",
  AccountName: '',
  SalesLedger: ''
})

const discountTotal = async (Quelity, Rate, DiscountTotal, gst) => {
  // total discount and Item rate
  discount.value = (ItemList.Rate * Quelity) - ((DiscountTotal / 100) * (Quelity * Rate))
  GstPlus.value = (gst / 100) * (discount.value)

  TotalAmount.value = discount.value + GstPlus.value
  // console.log('tTotalAmount.value ----> ', TotalAmount.value)
}

const totalQuality = ref(0);
const totalRate = ref(0);


const SaveProductDetails = async () => {
  await discountTotal(ItemList.Qlt, ItemList.Rate, ItemList.Discount, ItemList.Gst)

  console.log('log ->',applicants)

  // Test
  // applicants.filter((e) => {
  //   console.log('items details -----> ',e.Qlt)
  // })

  // for(let i = 0; i < applicants.length; i++){
  //   totalQuality.value = applicants[i].Qlt
  //   // console.log('totalQuality.value',totalQuality.value)
  //   // console.log('items details -----> ',applicants[i])
  // }

  totalQuality.value = applicants.reduce((acc, obj) => acc + obj.Qlt, 0);
  applicants.map((item,index) => {
    if(index)
    console.log('item',item,index)
  })

  const newArray = applicants.map(obj => {
    const discountPrice = obj.Rate * obj.Qlt * (1 - discount);
    const amount = discountPrice * (1 + obj.Gst / 100);
    return {
      ...obj,
      discountPrice,
      // percentage: discountPrice / totalPrice * 100,
      Amount: amount
    }
  });
  console.log('newArray', newArray)
  console.log('totalQuality1.value --->',totalQuality.value)
}

const count = ref(0)
const applicants = reactive([
  {
    id: count.value,
    itemName: '',
    Qlt: 0,
    Uom: '',
    Rate: 0,
    Gst: 0,
    Discount: 0,
    Amount: 0
  }
])

const  AddField = async () => {
  applicants.push({
    id: ++count.value,
    itemName: '',
    Qlt: 0,
    Uom: '',
    Rate: 0,
    Gst: 0,
    Discount: 0,
    Amount: 0
  })
}

// let check = [
//   {
//     itemName: '',
//     Qlt: 1,
//     Uom: '',
//     Rate: 100000,
//     Gst: 0,
//     Discount: 10,
//     Amount: 0
//   },
//   {
//     itemName: '',
//     Qlt: 2,
//     Uom: '',
//     Rate: 100000,
//     Gst: 18,
//     Discount: 5,
//     Amount: 0
//   }
// ]

const deleteField = (item) => {
  console.log('item --->', item)
  applicants.splice(item, 1)
}

</script>

<template>
  <div class='container-fluid py-2 '>
    <div class='row gap-2'>
      <div class='col'>
        <label for='InvoiceNo' class='form-label'>Invoice No</label>
        <input type='number' v-model='InvoiceDetails.InvoiceNumber' class='form-control' id='InvoiceNo'
               placeholder='001'>
      </div>
      <div class='col'>
        <label for='referenceNo' class='form-label'>Referance No.</label>
        <input type='number' v-model='InvoiceDetails.referenceNumber' class='form-control' id='referenceNo'
               placeholder='001'>
      </div>
      <div class='col'>
        <label for='Date' class='form-label'>Date</label>
        <input type='date' class='form-control' id='Date' placeholder='Thursday, 21-02-2019'>
      </div>
    </div>
    <div class='row'>
      <div class='col'>
        <label for='PartyName' class='form-label'>Party A/c Name</label>
        <input type='text' class='form-control' v-model='InvoiceDetails.AccountName' id='PartyName' placeholder='001'>
      </div>
      <div class='col'>
<!--        <p class='mb-2 common-text'>Sales Ledger</p>-->
                <label for='salesLadger' class='form-lab  el'>Sales Ledger</label>
        <select class='form-select' id='salesLadger' aria-label='Default select example'
                v-model='InvoiceDetails.SalesLedger'>
          <option selected>Default</option>
          <option value='1'>Amazon Sale</option>
          <option value='2'>Flipcart</option>
          <option value='3'>Alibaba</option>
        </select>
      </div>
    </div>
    <div class='row'>
      <div class='col'>
        <p class='m-0'>Address : Surat, Gujarat,</p>
        <p class='m-0'>Phone No. : 95687-69846 , Email id : abc@gmail.com</p>
      </div>
      <div class='col'>
        <p class='m-0'>Sale Account</p>
      </div>
    </div>
    <!--    item list-->
    <h4 class='py-2 text-black fs-4'>Item List</h4>
    <div class='row'>
      <div class='col-4'><span>Name of item</span></div>
      <div class='col-1'><span>Qty</span></div>
      <div class='col-1'><span>UOM</span></div>
      <div class='col-1'><span>Rate Per</span></div>
      <div class='col-1'><span>GST Rate</span></div>
      <div class='col-1'><span>Discount</span></div>
      <div class='col-1'><span>Amount</span></div>
    </div>
    <div v-for='applicant in applicants'>
      <form @submit.prevent='SaveProductDetails' v-on:input='SaveProductDetails'>
        <div class='row py-2'>
          <div class='col-4'>
<!--            <label for='NameItem' class='form-label'>Name of item</label>-->
            <input type='text' class='form-control' id='NameItem' v-model='applicant.itemName' placeholder='lorem ipsums'>
          </div>
          <div class='col-1'>
<!--            <label for='quality' class='form-label'>Qty</label>-->
<!--            <input type='number' class='form-control' id='quality' v-model='applicant.Qlt' placeholder='10'>-->
            <input type='number' class='form-control' id='quality' v-model='applicant.Qlt' placeholder='10'>
          </div>
          <div class='col-1'>
            <!--            <label for='uom' class='form-label'>UOM</label>-->
            <input type='number' class='form-control' id='uom' v-model='applicant.Uom' placeholder='MRTS'>
          </div>
          <div class='col-1'>
<!--            <label for='ratePer' class='form-label'>Rate Per</label>-->
            <input type='number' class='form-control' id='ratePer' v-model='applicant.Rate' placeholder='$30'>
          </div>
          <div class='col-1'>
<!--            <label for='gstRate' class='form-label'>GST Rate</label>-->
            <input type='number' class='form-control' v-model='applicant.Gst' id='gstRate' placeholder='10'>
          </div>
          <div class='col-1'>
<!--            <label for='discount' class='form-label'>Discount</label>-->
            <input type='number' class='form-control' id='discount' v-model='applicant.Discount' placeholder='10'>
          </div>
          <div class='col-2'>
<!--            <label for='Amount' class='form-label'>Amount</label>-->
            <input type='number' class='form-control' id='Amount' v-model='TotalAmount' placeholder='$10'>
          </div>
          <div class='col d-flex align-items-end'>
            <i class='fa-solid fa-pen fs-5 px-1' style='cursor: pointer '></i>
            <i class='fa-solid fa-trash fs-5 px-1' @click='deleteField(applicant)' style='cursor: pointer '></i>
            <i class='fa-solid fa-plus fs-5' style='cursor: pointer ' @click='AddField'></i>
          </div>
        </div>
      </form>
    </div>
    <div class='row'>
      <div class='col-4'>
        <p class='fw-bold py-3'>Total</p>
      </div>
      <div class='col-5'>
        <p class='fw-bold py-3'>{{ totalQuality === '' ? 0 : totalQuality }}</p>
      </div>
      <div class='col'>
        <p class='fw-bold py-3'>{{ TotalAmount }}</p>
      </div>
    </div>

    <div class='row'>
      <div class='col-8'>
        <p class='my-3 text-black fs-4'>Generate e-Way Bill</p>
        <div class='form-check form-check-inline'>
          <input class='form-check-input' type='checkbox' value='' id='flexCheckDefault'>
          <label class='form-check-label' for='flexCheckDefault'>
            No
          </label>
        </div>
        <div class='form-check form-check-inline'>
          <input class='form-check-input' type='checkbox' value='' id='GenarateEbook' checked>
          <label class='form-check-label' for='GenarateEbook'>
            Yes
          </label>
        </div>
        <div class='w-50 mt-3'>
          <label class='form-check-label' for='Narrotion'>
            Narration
          </label>
          <textarea class='form-control' placeholder='001' id='Narrotion'></textarea>
        </div>
      </div>
      <div class='col'>
        <div class='form-check'>
          <div class='d-flex align-items-center gap-2'>
            <input class='form-check-input' type='checkbox' value='' id='flexCheckChecked' checked>
            <span class='common-text'>Round Off</span>
            <div class='row'>
              <div class='col'>
                <input type='number' class='form-control' id='roundOffId' v-model='TotalAmount'>
              </div>
            </div>
          </div>
          <div class='row pt-3'>
            <div class='col-3 d-flex justify-content-start'>
              <span class='common-text'>CGST</span>
            </div>
            <div class='col'>
              <span class='common-text'>{{ GstPlus }}</span>
            </div>
          </div>
          <div class='row'>
            <div class='col-3 d-flex justify-content-start'>
<!--              1,000+ (1,000X(18/100)) = 1,000+180 = Rs. 1,180-->
              <span class='common-text'>SGST</span>
            </div>
            <div class='col'>
              <span class='common-text'>$1,23,567</span>
            </div>
          </div>
          <div class='row'>
            <div class='col-3 d-flex justify-content-start'>
              <span class='common-text'>IGST</span>
            </div>
            <div class='col'>
              <span class='common-text'>$1,23,567</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class='w-100 d-flex justify-content-end gap-4'>
      <button type='button' class='btn btn-outline-secondary px-5 rounded-pill'>Cancel</button>
      <button type='button' @click='SaveProductDetails' class='btn btn-primary px-5 rounded-pill'>Save</button>
    </div>
  </div>

<!--    <button @click='AddField'>Add Field</button>-->
<!--    <br>-->
<!--    <div class='previous' v-for='applicant in applicants'>-->
<!--      <span>{{ applicants.previous }}</span>-->
<!--      <label for='duration'>= Previous Visa:</label>-->
<!--      <input type='text' id='duration' v-model='applicant.previous' required>-->
<!--      <label for='duration1'>Year of expiration:</label>-->
<!--      <input type='text' id='duration1' v-model='applicant.expiration' required>-->
<!--    </div>-->
</template>

<style scoped>

</style>
