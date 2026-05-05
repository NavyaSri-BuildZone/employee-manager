<template>
  <div class="bg">
    <div class="container py-5">

```
  <div class="card shadow-lg p-4 rounded-4">
    <h2 class="text-center mb-4 fw-bold text-purple">
      Employee Manager
    </h2>

    <!-- FORM -->
    <div class="row g-3 mb-4">
      <div class="col-md-3">
        <input v-model="emp.name" class="form-control rounded-3" placeholder="Name">
      </div>
      <div class="col-md-3">
        <input v-model="emp.designation" class="form-control rounded-3" placeholder="Designation">
      </div>
      <div class="col-md-3">
        <input v-model="emp.department" class="form-control rounded-3" placeholder="Department">
      </div>
      <div class="col-md-2">
        <input v-model="emp.salary" class="form-control rounded-3" placeholder="Salary">
      </div>
      <div class="col-md-1 d-grid">
        <button class="btn btn-purple rounded-3" @click="saveEmp">
          {{ editMode ? '✔' : '+' }}
        </button>
      </div>
    </div>

    <!-- TABLE -->
    <div class="table-responsive">
      <table class="table table-hover align-middle text-center">
        <thead class="table-light">
          <tr>
            <th>Name</th>
            <th>Designation</th>
            <th>Department</th>
            <th>Salary</th>
            <th>Actions</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="e in employees" :key="e.id">
            <td>{{ e.name }}</td>
            <td>{{ e.designation }}</td>
            <td>{{ e.department }}</td>
            <td>₹ {{ e.salary }}</td>
            <td>
              <button class="btn btn-sm btn-outline-warning me-2" @click="editEmp(e)">Edit</button>
              <button class="btn btn-sm btn-outline-danger" @click="deleteEmp(e.id)">Delete</button>
            </td>
          </tr>

          <tr v-if="employees.length === 0">
            <td colspan="5" class="text-muted">No employees found</td>
          </tr>
        </tbody>
      </table>
    </div>

  </div>

</div>
```

  </div>
</template>

<script>
import axios from "axios";

const API = "https://69f95a84c509a40d3aa25bb5.mockapi.io/employees";

export default {
  data() {
    return {
      employees: [],
      emp: {
        name: "",
        designation: "",
        department: "",
        salary: ""
      },
      editMode: false,
      editId: null
    };
  },

  methods: {
    async getData() {
      const res = await axios.get(API);
      this.employees = res.data;
    },

    async saveEmp() {
      try {
        if (!this.emp.name) {
          alert("Enter name");
          return;
        }

        if (this.editMode && this.editId) {
          await axios.put(`${API}/${this.editId}`, this.emp);
        } else {
          await axios.post(API, this.emp);
        }

        this.editMode = false;
        this.editId = null;

        this.reset();
        await this.getData();

      } catch (err) {
        console.error(err);
      }
    },

    editEmp(e) {
      this.emp = { ...e };
      this.editId = e.id;
      this.editMode = true;
    },

    async deleteEmp(id) {
      try {
        await axios.delete(`${API}/${id}`);
        await this.getData();
      } catch (err) {
        console.error(err);
      }
    },

    reset() {
      this.emp = {
        name: "",
        designation: "",
        department: "",
        salary: ""
      };
    }
  },

  mounted() {
    this.getData();
  }
};
</script>

<style>
.bg {
  background: linear-gradient(135deg, #f3e7ff, #e0f7ff);
  min-height: 100vh;
}

.text-purple {
  color: #7b3fe4;
}

.btn-purple {
  background-color: #7b3fe4;
  color: white;
}

.btn-purple:hover {
  background-color: #5e2fc0;
}

.card {
  border: none;
}
</style>
