<template>
    <div>
        <div class="row">
            <form @submit.prevent="save">
                
                <div class="form-group">
                    <label for="exampleInputEmail1">Adı Soyadı</label>
                    <input
                        type="text"
                        v-model="employee.name"
                        class="form-control"
                        name="name"
                        placeholder="Enter Name"
                    />
                </div>
                <div class="form-group">
                    <label for="exampleInputEmail1">Adres</label>
                    <input
                        type="address"
                        v-model="employee.address"
                        class="form-control"
                        name="address"
                        placeholder="Address"
                    />
                </div>

                <div class="form-group">
                    <label for="exampleInputEmail1">Telefon</label>
                    <input
                        type="phone"
                        v-model="employee.mobile"
                        class="form-control"
                        name="mobile"
                        placeholder="Phone"
                    />
                </div>
                
                <button v-if="employee.id == ''" type="submit" class="btn btn-primary">Kaydet</button>
                <button v-else type="submit" class="btn btn-primary">Güncelle</button>
            </form>
        </div>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Adı Soyadı</th>
                    <th scope="col">Addres</th>
                    <th scope="col">Telefon</th>
                    <th scope="col">Statu</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in result" :key="employee.id">
                    <th scope="row">{{ employee.id }}</th>
                    <td>{{ employee.name }}</td>
                    <td>{{ employee.address }}</td>
                    <td>{{ employee.mobile }}</td>
                    <td>
                        <button
                            type="submit"
                            class="btn btn-warning"
                            @click="edit(employee)"
                        >
                            Edit
                        </button>
                        <button
                            type="submit"
                            class="btn btn-danger"
                            @click="remove(employee)"
                        >
                            Sil
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: "Employee",
    data() {
        return {
            result: {},

            employee: {
                id: "",
                name: "",
                address: "",
                mobile: "",
            },
        };
    },

    created() {
        this.employeeView();
    },

    methods: {
        employeeView() {
            var page = "http://localhost:8000/api/employee";
            axios.get(page).then(({ data }) => {
                console.log(data);

                this.result = data;
            });
        },

        save() {
            if (this.employee.id == "") {
                this.saveData();
            } else {
                this.updateData();
            }
        },

        saveData() {
            axios
                .post("http://127.0.0.1:8000/api/save", this.employee)
                .then(({ data }) => {
                    alert("Savedd");
                    this.employeeView();
                });
        },
        edit(employee) {
            this.employee = employee;
        },

        updateData() {
            var editrecords =
                "http://localhost:8000/api/update/" + this.employee.id;
            axios
                .put(editrecords, this.employee)

                .then(({ data }) => {
                    this.employee.name = "";
                    this.employee.address = "";
                    this.employee.mobile = "";
                    this.employee.id = "";
                    alert("updated!!");

                    this.employeeView();
                });
        },

        remove(employee) {
            var url = "http://localhost:8000/api/delete/" + employee.id;

            axios.delete(url);
            alert("Deleted");
            this.employeeView();
        },
    },
};
</script>

<style></style>
