<template>
<div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">Edit Contact</p>
            <p class="fst-italic">It is a long established fact that a reader will be
                by the readable content of a page when looking at its layout. The point
                of using Lorem Ipsum is that it has a more-or-less normal distribution
                of letters, as opposed to using 'Content here, content here', making it
                look like readable English. Many desktop publishing packages and web
            </p>
        </div>
    </div>
</div>

<div class="container mt-3">
    <div class="row">
        <div class="col-md-4">
            <form @submit.prevent="updateSubmit()">
                <div class="mb-2">
                    <input required type="text" v-model="contact.name" class="form-control" placeholder="name">
                </div>
                <div class="mb-2">
                    <input required type="text" v-model="contact.photo" class="form-control" placeholder="Photo URL">
                </div>
                <div class="mb-2">
                    <input required type="email" v-model="contact.email" class="form-control" placeholder="Email">
                </div>
                <div class="mb-2">
                    <input required type="number" v-model="contact.mobile" class="form-control" placeholder="Mobile">
                </div>
                <div class="mb-2">
                    <input required type="text" v-model="contact.company" class="form-control" placeholder="Company">
                </div>
                <div class="mb-2">
                    <input required type="text" v-model="contact.title" class="form-control" placeholder="Title">
                </div>
                <div class="mb-2">
                    <select required v-model="contact.groupId" class="form-control" v-if="groups.length > 0">
                        <option value="">Select Group</option>
                        <option :value="group.id" v-for="group of groups" :key="group.id">{{ group.name }}</option>
                    </select>

                </div>
                <div class="mb-2">
                    <input type="submit" class="btn btn-success" value="Update">
                </div>
            </form>
        </div>
        <div class="col-md-4">
            <img :src="contact.photo" alt="" class="contact-img">
        </div>
    </div>
</div>
</template>

<script>
import {
    ContactService
} from '../services/ContactService';

export default {
    name: "EditContact",
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {
                name: '',
                photo: '',
                email: '',
                mobile: '',
                company: '',
                title: '',
                groupId: ''

            },
            errorMessage: null,
            group: []
        }
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContact(this.contactId);
            let getResponse = await ContactService.getAllGroups();
            this.contact = response.data;
            this.groups = getResponse.data;
            this.errorMessage = false;
        } catch (error) {
            this.errroMessage = error;
            this.loading = false;

        }

    },
    methods: {
        updateSubmit: async function () {

            try {
                let response = await ContactService.updateContact(this.contact, this.contactId);
                if (response) {
                    return this.$router.push('/');

                } else {
                    return this.$router.push(`/contacts/edit/${this.contactId}`);
                }
            } catch (error) {
                console.log(error);

            }

        }
    }
}
</script>

<style scoped>

</style>
