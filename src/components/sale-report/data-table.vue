<template>
<LayoutDefault>

    <div>
        <!-- <HeaderBar/> -->
      <b-row>
        <b-alert v-model="showSuccessAlert" variant="success" dismissible>
          {{ alertMessage }}
        </b-alert>
      </b-row>
      <b-row>
        <overview
          :totalCustomers="numberOfCustomers"
          :activeCustomers="activeCustomers"
        ></overview>
      </b-row>
      <b-row class="mt-3">
        <b-card>
          <b-row align-h="between">
            <b-col cols="6">
              <h3>{{ tableHeader }}</h3>
            </b-col>
            <b-col cols="2">
              <b-row>
                <b-col>
                  <b-button
                    variant="primary"
                    id="show-btn"
                    @click="showCreateModal"
                  >
                    <b-icon-plus class="text-white"></b-icon-plus>
                    <span class="h6 text-white">New Employee</span>
                  </b-button>
                </b-col>
              </b-row>
            </b-col>
          </b-row>
          <b-row class="mt-3">
            <b-table
              striped
              hover
              :items="items"
              :fields="fields"
              class="text-center"
            >
              <template #cell(contact_name)="data">
                {{
                  `${data.item.name} ${data.item.model}`
                }}
              </template>
              <template #cell(customer_status)="data">
                <b-icon-bookmark-check-fill
                  variant="success"
                  v-if="data.item.customer_status === ''"
                ></b-icon-bookmark-check-fill>
                <b-icon-bookmark-x-fill
                  variant="danger"
                  v-else
                ></b-icon-bookmark-x-fill>
              </template>
              <template #cell(actions)="data">
                <b-row>
                  <b-col cols="7">
                    <b-icon-pencil-square
                      class="action-item"
                      variant="primary"
                      @click="getRowData(data.item.id)"
                    ></b-icon-pencil-square>
                  </b-col>
                <b-col cols="1">
                    <b-icon-trash-fill
                      class="action-item"
                      variant="danger"
                      @click="showDeleteModal(data.item.id)"
                    ></b-icon-trash-fill>
                  </b-col>
                </b-row>
              </template>
            </b-table>
          </b-row>
        </b-card>
      </b-row>
  
      <!-- Modal for adding new customers -->
      <b-modal
        ref="create-customer-modal"
        size="xl"
        hide-footer
        title="New Employee"
      >
        <create-form
          @closeCreateModal="closeCreateModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertCreate"
        ></create-form>
      </b-modal>
  
      <!-- Modal for updating customers -->
      <b-modal
        ref="edit-customer-modal"
        size="xl"
        hide-footer
        title="Edit Employee"
      >
        <edit-form
          @closeEditModal="closeEditModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertUpdate"
          :customerId="customerId"
        ></edit-form>
      </b-modal>
  
      <!-- Delete Customer Modal -->
      <b-modal
        ref="delete-customer-modal"
        size="md"
        hide-footer
        title="Confirm Deletion"
      >
        <delete-modal
          @closeDeleteModal="closeDeleteModal"
          @reloadDataTable="getCustomerData"
          @showDeleteAlert="showDeleteSuccessModal"
          :customerId="customerId"
        ></delete-modal>
      </b-modal>
    </div></LayoutDefault>
  </template>
  
  <script>
  import axios from "axios";
  import Overview from "./Overview.vue";
  import CreateForm from "./CreateForm.vue";
  import EditForm from "./EditForm.vue";
  import DeleteModal from "./DeleteModal.vue";
  import LayoutDefault from "@/layouts/LayoutDefault.vue";

  export default {
    components: {
      Overview,
      CreateForm,
      EditForm,
      DeleteModal,
      LayoutDefault 
    },
    data() {
      return {
        // Note 'isActive' is left out and will not appear in the rendered table

        // "empFirstName": "devaon",
        // "empLastName": "ouh",
        // "empPhoneNumber": "0727408325",
        // "empRole": "manager",
        
  
        fields: [
          {
            key: "empID",
            label: "ID",
            sortable: false,
          },
          {
            key: "empRole",
            label: "Employee Role",
            sortable: false,
          },
          {
            key: "empPhoneNumber",
            label: "Employee Phone Number",
            sortable: false,
          },
          {
            key: "empLastName",
            label: "Employee LastName",
            sortable: false,
          },
          {
            key: "empFirstName",
            label: "Employee Fist Name",
            sortable: false,
          },
       
       
          "actions",
        ],
        items: [],
        numberOfCustomers: 0,
        activeCustomers: 0,
        activeCustomersData: [],
        customerId: 0,
        companySearchTerm: "",
        tableHeader: "",
        showSuccessAlert: false,
        alertMessage: "",
      };
    },
    mounted() {
      this.getCustomerData();
    },
    methods: {
      showCreateModal() {
        this.$refs["create-customer-modal"].show();
      },
      closeCreateModal() {
        this.$refs["create-customer-modal"].hide();
      },
      getCustomerData() {
        axios
          .get("http://localhost:8083/api/employee/all")
          .then((response) => {
            this.tableHeader = "Total Employee";
            this.items = response.data;
            this.numberOfCustomers = response.data.length;
          
          })
          .catch((error) => {
            console.log(error);
          });
      },
      getRowData(id) {
        this.$refs["edit-customer-modal"].show();
        this.customerId = id;
      },
      closeEditModal() {
        this.$refs["edit-customer-modal"].hide();
      },
      setFilterTotalIsActive() {
        this.tableHeader = "Total ";
        this.getCustomerData();
      },
      setFilterActiveIsActive() {
        this.tableHeader = "Active Customers";
        this.items = this.activeCustomersData;
      },
      showAlertCreate() {
        this.showSuccessAlert = true;
        this.alertMessage = "Employee was created successfully!";
      },
      showAlertUpdate() {
        this.showSuccessAlert = true;
        this.alertMessage = "Employee was updated successfully";
      },
      showDeleteModal(id) {
        this.$refs["delete-customer-modal"].show();
        this.customerId = id;
      },
      closeDeleteModal() {
        this.$refs["delete-customer-modal"].hide();
      },
      showDeleteSuccessModal() {
        this.showSuccessAlert = true;
        this.alertMessage = "Employee  was deleted successfully!";
      },
    },
  };
  </script>
  
  <style>
  .action-item:hover {
    cursor: pointer;
  }
  </style>