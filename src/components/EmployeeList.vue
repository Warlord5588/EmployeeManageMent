<template>
    <div>
      <!-- nav bar section -->
      <v-app-bar app color="white">
        <v-img src="https://cdn.vuetifyjs.com/images/parallax/material.jpg" :width="50" cover></v-img>
        <v-toolbar-title></v-toolbar-title>
        <v-spacer></v-spacer>
  
        <div style="position: absolute; width: 204px; height: 36px; right: 309px; top: 5px;">
            <v-text-field v-model="search" label="Search" @input="searchEmployees"></v-text-field>>
        </div>

        <div style="position: absolute; width: 1px; height: 36px; right: 284px; top: 18px; background: #E8E8EA;"/>
        
        <div style="position: absolute; width: 28px; height: 28px; right: 232px; top: 12px;"><v-btn icon="mdi-chat-processing-outline"/></div>
        <div style="position: absolute; width: 28px; height: 28px; right: 180px; top: 12px;"><v-btn icon="mdi-bell-circle-outline"/></div>
        <div style="position: absolute; width: 28px; height: 28px; right: 128px; top: 12px;"><v-btn icon="mdi-help-circle-outline"/></div>
        <div style="position: absolute; width: 28px; height: 28px; right: 76px; top: 12px;"><v-btn icon="mdi-apps"/></div>
        <div style="position: absolute; width: 28px; height: 28px; right: 24px; top: 12px;"><v-btn icon="mdi-account-circle"/></div>
        
      </v-app-bar>
      <!-- nav bar section -->
      
      <!-- drawer section -->
      <v-navigation-drawer v-model="drawer" app color="black">
        <v-list>
          <v-list-item v-for="item in menuItems" :key="item.title" link>
            <component :is="'v-list-item-icon'">
              <v-icon>{{ item.icon }}</v-icon>
            </component>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>
      <!-- drawer section -->
  
      <!-- content section -->
      <component :is = "'v-content'">
        <v-container>
          <v-btn icon @click="toggleMenu">
          <v-icon>{{ menuIcon }}</v-icon>
        </v-btn>
        <br>
        <br>
  
        <v-select v-model="department" :items="departments" label="Department" outlined dense />
              
      <v-card>
        <v-card-text>
            <template v-slot:default>
  
                      Name
                      Department
                      Salary
                      Hired Date
  
                  <br>
                  <draggable v-model="visibleEmployees" handle=".drag-handle">
                  <v-card v-for="employee in visibleEmployees" :key="employee.id" class="drag-handle">
                <v-card-text>
                  {{ employee.name }}
                  {{ employee.department }}
                  {{ employee.salary }}
                  {{ employee.hiredDate }}
                 </v-card-text>
              </v-card>
            </draggable>
                  <br>
            </template>
            </v-card-text>
            <v-pagination v-model="currentPage" :total-visible="10" :items-per-page="10" :length="totalPages" @input="updateVisibleEmployees" />
      </v-card>
        </v-container>
      </component>
      <!-- content section -->
  
    </div>
  
  
  </template>
  
  <script>
  import { employees } from '@/data.js'
  
  export default {
    data() {
      return {
        drawer: false,
        employees: employees,
        search: '',
        department: '',
        sort: '',
        currentPage: 1,
        menuItems: [
          { title: 'Home', icon: 'mdi-home' },
          { title: 'About', icon: 'mdi-help-circle' },
          { title: 'Contact', icon: 'mdi-phone' },
        ],
        departments: [...new Set(employees.map((employee) => employee.department))],
      }
    },
    computed: {
      menuIcon() {
        return this.drawer ? 'mdi-close' : 'mdi-menu'
      },
      filteredEmployees() {
        let employees = this.employees
        if (this.search) {
          employees = employees.filter((employee) =>
            employee.name.toLowerCase().includes(this.search.toLowerCase())
          )
        }
        if (this.department) {
          employees = employees.filter(
            (employee) => employee.department === this.department
          )
        }
        return employees
      },
      totalPages() {
        return Math.ceil(this.filteredEmployees.length / 10)
      },
      visibleEmployees() {
        const start = (this.currentPage - 1) * 10
        const end = start + 10
        return this.filteredEmployees.slice(start, end)
      }
    },
    methods: {
      toggleMenu() {
        this.drawer = !this.drawer
      },
      updateVisibleEmployees() {
        this.currentPage = 1
      },
      searchEmployees() {
        this.currentPage = 1
      }
    },
    mounted() {
      if (localStorage.getItem('menu') === 'true') {
        this.drawer = true
      }
    },
    watch: {
      drawer(newVal) {
        localStorage.setItem('menu', newVal)
      },
    },
  }
  </script>
  
  <style>
  .drag-handle {
    cursor: move;
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px;
    background-color: #fff;
  }
  </style>