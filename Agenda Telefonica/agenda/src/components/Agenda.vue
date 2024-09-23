<template>
    <nav class="navbar bg-body-tertiary" >
        <div class="nav-btns">
            <button type="button" class="btn btn-primary" @click="newNumberWindow()">Add Number</button>
            <button type="button" class="btn btn-danger" @click="clearstorage()">Clear everything</button>
        </div>
    </nav>

    <table class="table table-striped table-hover"  @load = "showOnLoad()"> 
        <thead>
            <tr>
                <td>Name</td>
                <td>Number</td>
            </tr>
        </thead>
        <tbody>
            <tr v-for="x in numbers">
                <td class="listName">{{ x.nume }}</td>
                <td class="listNumber">{{ x.numar }}</td>
                <td class="listDelete"><button type="button" class="btn btn-danger"
                        @click="deleteNumber(x.numar)">Delete</button>
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
        <div class="buttons">
            <button type="button" class="btn btn-success form-button save-btn" @click="newNumber()">Save New Number</button>
        </div>
    </form>
</template>

<script>
export default {
    name: 'Agenda',

    data() {
        return {
            showInfo: false,
            person: {
                id: 0,
                name: "",
                phone: "",
            },
            numbers: [],
            numberCount: 0

        }
    },

    methods: {
        showOnLoad() {
            for(let i=0; i < localStorage.length; i++){
                this.person.name = localStorage[i].split(',')[0].split('"')[3];
                this.person.phone = localStorage[i].split(',')[1].split('"')[3];
                this.addNumberToList();
            }
        },

        clearstorage() {
            localStorage.clear();
            this.numbers = [];
        },

        clearPerson() {
            this.person.name = '';
            this.person.phone = '';
        },

        newNumberWindow() {
            this.showInfo = true;
            this.notClickable = true;
        },

        newNumber() {
            this.showInfo = true;
            if(this.numbers.length == 0){
                this.addNumberToList();
            }else if (this.numberExists(this.person.phone)) {
                console.log('number exists');
            }else{
                this.addNumberToList();
            }
        },

        addNumberToList(){
            var data = {
                    nume: this.person.name,
                    numar: this.person.phone,
                }
                this.numbers.push(data);
                this.addNumberToStorage(data);
                this.showInfo = false;
                this.clearPerson();
                this.numberCount += 1;
        },

        addNumberToStorage(data) {
            let storageData = JSON.stringify(data);
            localStorage.setItem(this.numberCount, storageData);

        },

        refreshStorage(data) {
            for (let i = 0; i < data.length; i++) {
                localStorage.setItem(i, JSON.stringify(data[i]))
            }
        },

        deleteNumber(numarDel) {
            localStorage.clear();
            for (let i = 0; i < this.numbers.length; i++) {
                if (numarDel == this.numbers[i].numar) {
                    this.numbers.splice(i, 1);
                }
            }
            this.refreshStorage(this.numbers);
            this.numberCount -= 1;
        },

        numberExists(number) {
            for (let i = 0; i < this.numbers.length; i++) {
                if (number == this.numbers[i].numar) {
                    alert('Numarul exista deja!');
                    this.showInfo = false;
                    this.clearPerson();
                    return true;
                }
            }
        }

    },

    mounted: function(){
        this.showOnLoad();
    }
}

</script>

<style scoped>
nav {
    font-size: 2rem;
    width: 50%;
}

.nav-btns {
    width: 100%;
    margin-right: 1%;
}

.nav-btns .btn-danger {
    margin-left: 70%;
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
</style>