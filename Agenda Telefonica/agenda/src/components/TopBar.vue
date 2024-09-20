<template>
    <nav class="navbar bg-body-tertiary">
        <form class="d-flex" role="search">
            <label for="search">Search:</label>
            <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" name="search">
            <button class="btn btn-outline-dark" type="submit">Search</button>
        </form>
        <div class="nav-btns">
            <button type="button" class="btn btn-primary" @click="newNumberWindow()">Add Number</button>
            <button type="button" @click="clearstorage()">clear</button>
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
            <tr v-for="x in numbers">
                <td class="listID">#{{ x.id }}</td>
                <td class="listName">{{ x.nume }}</td>
                <td class="listNumber">{{ x.numar }}</td>
                <td class="listEdit"><button type="button" class="btn btn-info"
                        @click="editNumberWindow(x.id)">Edit</button></td>
                <td class="listDelete"><button type="button" class="btn btn-danger" @click="test(x.id + 1)">Delete</button>
                </td>
            </tr>
        </tbody>

    </table>

    <form id="nameInfo" v-show="showInfo">
        <div class="name">
            <label for="name" class="form-label">Name:</label>
            <input type="text" id="name" for="name" class="input" v-model="person.name">
        </div>
        <div class="phoneNumber">
            <label for="number" class="form-label">Phone:</label>
            <input type="tel" id="phoneNumber" for="phoneNumber" class="input" v-model="person.phone">
        </div>
        <div class="phoneNumberSecond">
            <label for="number" class="form-label">Phone 2:</label>
            <input type="tel" id="phoneNumber" for="phoneNumber" class="input" v-model="person.secondPhone">
        </div>
        <div class="email">
            <label for="email" class="form-label">E-mail:</label>
            <input type="email" id="email" for="email" class="input" v-model="person.email">
        </div>
        <div class="buttons">
            <button type="button" class="btn btn-success form-button save-btn" @click="newNumber()">Save New Number</button>
            <button type="button" class="btn btn-danger form-button delete-btn" @click="deleteNumber(this.currentId)">Delete
                Number</button>
            <button type="button" class="btn btn-primary form-button duplicate-btn" :disabled='notClickable'
                @click="editNumber(this.currentId)">Save Changes</button>
        </div>
    </form>
</template>

<script>
export default {
    name: 'TopBar',

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
            numbers: [],
            currentId: 0,
            notClickable: true,
        }
    },

    methods: {
        clearstorage() {
            localStorage.clear();
        },
        newNumberWindow() {
            this.showInfo = true;
            this.notClickable = true;
        },

        clearPerson() {

            this.person.name = '',
                this.person.phone = '',
                this.person.secondPhone = '',
                this.person.email = ''

        },


        newNumber() {
            this.person.id += 1;
            var data = {
                id: this.person.id,
                nume: this.person.name,
                numar: this.person.phone,
                numarDoi: this.person.secondPhone,
                email: this.person.email,
            }
            this.clearPerson();
            this.numbers.push(data);
            this.addNumber(data);
            this.showInfo = false;
        },

        addNumber(data) {
            let storageData = JSON.stringify(data);
            localStorage.setItem(data.id, storageData);
        },

        editNumberWindow(id) {
            this.currentId = id;
            this.showInfo = true;
            this.notClickable = false;
            this.person.name = this.numbers[id - 1].nume;
            this.person.phone = this.numbers[id - 1].numar;
            this.person.secondPhone = this.numbers[id - 1].numarDoi;
            this.person.email = this.numbers[id - 1].email;
        },

        editNumber(id) {
            this.numbers[id - 1].nume = this.person.name;
            this.numbers[id - 1].numar = this.person.phone;
            this.numbers[id - 1].numarDoi = this.person.secondPhone;
            this.numbers[id - 1].email = this.person.email;
            localStorage.setItem(id, JSON.stringify(this.numbers[id - 1]));
            this.showInfo = false;
        },

        deleteNumber(id) {
            this.person.id = this.person.id - 1;
            this.numbers.splice(id - 1, 1);
            for (let i = 0; i <= this.numbers.length; i++) {
                if (this.numbers[i].id > id) {
                    this.numbers[i].id = this.numbers[i].id - 1;
                }
            }

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