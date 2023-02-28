<template>
    <form @submit.prevent="submitForm">
        <div class="form-control" :class="{ invalid: !firstName.isValid }">
            <label for="firstname">Firstname</label>
            <input type="text" id="firstname" v-model.trim="firstName.val" />
            <p v-if="!firstName.isValid">Firstname must not be empty.</p>
        </div>
        <div class="form-control" :class="{ invalid: !lastName.isValid }">
            <label for="lastname">Lastname</label>
            <input type="text" id="lastname" v-model.trim="lastName.val" />
            <p v-if="!lastName.isValid">Lastname must not be empty.</p>
        </div>
        <div class="form-control" :class="{ invalid: !description.isValid }">
            <label for="description">Description</label>
            <textarea type="text" id="description" rows="5" v-model.trim="description.val"></textarea>
            <p v-if="!description.isValid">Description must not be empty.</p>
        </div>
        <div class="form-control" :class="{ invalid: !rate.isValid }">
            <label for="rate">Hourly Rate</label>
            <input type="number" id="rate" v-model="rate.val" />
            <p v-if="!rate.isValid">Hourly Rate must be greater than 0.</p>
        </div>
        <div class="form-control" :class="{ invalid: !areas.isValid }">
            <h3>Areas of Expertise</h3>
            <div class="checkbox-container">
                <input type="checkbox" id="frontend" value="frontend" v-model="areas.val" />
                <label for="frontend">Frontend Development</label>
            </div>
            <div class="checkbox-container">
                <input type="checkbox" id="backend" value="backend" v-model="areas.val" />
                <label for="backend">Backend Development</label>
            </div>
            <div class="checkbox-container">
                <input type="checkbox" id="career" value="career" v-model="areas.val" />
                <label for="career">Career Advisory</label>
            </div>
            <p v-if="!areas.isValid">At least one expertise must be selected.</p>

        </div>
        <p v-if="!formIsValid">Please fix the above errors and submit again</p>
        <base-button>Register</base-button>
    </form>
</template>

<script>
export default {
    emits: ['save-data'],
    data() {
        return {
            firstName: {
                val: '',
                isValid: true
            },
            lastName: {
                val: '',
                isValid: true
            },
            description: {
                val: '',
                isValid: true
            },
            rate: {
                val: null,
                isValid: true
            },
            areas: {
                val: [],
                isValid: true
            },
            formIsValid: true
        };
    },
    methods: {
        validateForm() {
            this.formIsValid = true;
            if (this.firstName.val === '') {
                this.firstName.isValid = false;
                this.formIsValid = false;
            }
            if (this.lastName.val === '') {
                this.lastName.isValid = false;
                this.formIsValid = false;
            }
            if (this.description.val === '') {
                this.description.isValid = false;
                this.formIsValid = false;
            }
            if (!this.rate.val || this.rate.val < 0) {
                this.rate.isValid = false;
                this.formIsValid = false;
            }
            if (this.areas.val.length === 0) {
                this.areas.isValid = false;
                this.formIsValid = false;
            }
        },
        submitForm() {
            this.validateForm();

            if (!this.formIsValid) {
                return;
            }

            const formData = {
                first: this.firstName,
                last: this.lastName,
                desc: this.description,
                rate: this.rate,
                area: this.areas
            }

            this.$emit('save-data', formData)
        }
    }
}
</script>

<style scoped>
.form-control {
    margin: 0.5rem 0;
}

input {
    border-radius: 5px;
}

.checkbox-container {
    display: flex;
}

label {
    font-weight: bold;
    display: block;
    margin-bottom: 0.5rem;
}

input[type='checkbox']+label {
    font-weight: normal;
    display: inline;
    margin: 0 0 0 0.5rem;
}

input,
textarea {
    display: block;
    width: 100%;
    border: 1px solid #ccc;
    font: inherit;
}

input:focus,
textarea:focus {
    background-color: #f0e6fd;
    outline: none;
    border-color: #3d008d;
}

input[type="checkbox"] {
    position: relative;
    width: 40px;
    height: 20px;
    -webkit-appearance: none;
    background: #c6c6c6;
    outline: none;
    top: -2px;
    border-radius: 20px;
    box-shadow: inset 0 0 5px rgba(255, 0, 0, 0.2);
    transition: 0.7s;
}

input:checked[type="checkbox"] {
    background: #3d008d;
}

input[type="checkbox"]:before {
    content: '';
    position: absolute;
    width: 20px;
    height: 20px;
    border-radius: 20px;
    top: 0;
    left: 0;
    background: #ffffff;
    transform: scale(1.1);
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    transition: .5s;
}

input:checked[type="checkbox"]:before {
    left: 20px;
}

h3 {
    margin: 0.5rem 0;
    font-size: 1rem;
}

.invalid label {
    color: red;
}

.invalid input,
.invalid textarea {
    border: 1px solid red;
}
</style>