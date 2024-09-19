<template>
    <nav class="navbar bg-body-tertiary">
        <form class="d-flex" role="search">
            <label for="search">Search:</label>
            <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" name="search">
            <button class="btn btn-outline-dark" type="submit">Search</button>
        </form>
        <div class="nav-btns">
            <button type="button" class="btn btn-primary" @click="testShow()">Add Number</button>
        </div>
    </nav>

    <table class="table table-striped table-hover">
        <thead>
            <tr>
                <td>ID</td>
                <td>Name</td>
                <td>Number</td>
            </tr>
        </thead>
        <tbody>
            <TableRow v-for="x in numbers" v-bind:id="x.id" v-bind:name="x.nume" v-bind:phone="x.numar" @click=test(x.id) />
        </tbody>
    </table>

    <form id="nameInfo" v-show="showInfo">
        <div class="name">
            <label for="name" class="form-label">Name:</label>
            <input type="text" id="name" for="name" class="input" placeholder="asd" v-model="person.name">
        </div>
        <div class="phoneNumber">
            <label for="number" class="form-label">Phone:</label>
            <input type="tel" id="phoneNumber" for="phoneNumber" class="input" placeholder="1234" v-model="person.phone">
        </div>
        <div class="phoneNumberSecond">
            <label for="number" class="form-label">Phone 2:</label>
            <input type="tel" id="phoneNumber" for="phoneNumber" class="input" placeholder="1234"
                v-model="person.secondPhone">
        </div>
        <div class="email">
            <label for="email" class="form-label">E-mail:</label>
            <input type="email" id="email" for="email" class="input" placeholder="1234" v-model="person.email">
        </div>
        <div class="buttons">
            <button type="button" class="btn btn-success form-button save-btn" @click="saveNumber()">Save Number</button>
            <button type="button" class="btn btn-danger form-button delete-btn">Delete Number</button>
            <button type="button" class="btn btn-primary form-button duplicate-btn">Duplicate Number</button>
        </div>
    </form>
</template>

<script>
import TableRow from './TableRow.vue';
export default {
    name: 'TopBar',

    components: {
        TableRow: TableRow
    },

    data() {
        return {
            showInfo: false,
            person: {
                id: 0,
                name: "",
                phone: "",
                secondPhone: "",
                email: "",
            },
            numbers: []
        }
    },

    methods: {
        testShow() {
            this.showInfo = true;
        },

        test(id) {
            console.log(`test from ${id}`);
        },

        saveNumber() {
            this.person.id += 1;
            var data = {
                id: this.person.id,
                nume: this.person.name,
                numar: this.person.phone,
                numarDoi: this.person.secondPhone,
                email: this.person.email,
            }
            this.addNumber(data);
        },

        addNumber(data) {
            this.numbers.push(data);
            let storageData = JSON.stringify(data);
            localStorage.setItem(data.id, storageData);
            console.log(this.numbers);
            console.log(localStorage.getItem(1));

        }
    }
}

</script>

<style scoped>
nav {
    font-size: 2rem;
    width: 50%;
}

.nav-btns {
    margin-right: 1%;
}

nav label {
    margin-right: 5%;
}

.table {
    font-size: 2rem;
    width: 50%;
    margin-left: 0%;
}

.listEdit,
.listDelete {
    width: 0;
}

#nameInfo {
    width: 30%;
    float: right;
    padding: 0.3%;
    font-size: 2rem;
    margin-right: 10%;
    border-color: black;
    border-width: 1px;
    border-style: solid;
    border-radius: 5px;

}

#nameInfo .input {
    padding: 0 1%;
    border-radius: 5px;
    border-width: 1px;
    margin-left: 2%;
    float: right;
}

.save-btn {
    float: left;
}

.delete-btn {
    float: left;
    margin-left: 12%;
}

.duplicate-btn {
    float: right;
}
</style>