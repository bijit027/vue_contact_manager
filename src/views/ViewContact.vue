<template>
<div class="container mt-3">
    <div class="row">
        <div class="col">
            <p class="h3 text-success fw-bold">View Contact</p>
            <p class="fst-italic">
                It is a long established fact that a reader will be
                by the readable content of a page when looking at its layout. The point
                of using Lorem Ipsum is that it has a more-or-less normal distribution
                of letters, as opposed to using 'Content here, content here', making it
                look like readable English. Many desktop publishing packages and web
            </p>
        </div>
    </div>
</div>
<!--Spinner-->
<div v-if="loading">
    <div class="container">
        <div class="row">
            <div class="col">
                <SpinnerComponents />
            </div>
        </div>
    </div>

</div>

<!--Error Message-->
<div v-if="!loading && errorMessage">
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h4 text-danger fw-bold ">{{ errorMessage }}</p>
            </div>
        </div>

    </div>
</div>
<div class="container">
    <div class="row align-items-center">
        <div class="col-md-4">
            <img :src="contact.photo" alt="" class="contact-img-big">
        </div>
        <div class="col-md-6 ">
            <ul>
                <li class="list-group-item">
                    Name : <span class="fw-bold">{{ contact.name }}</span>
                </li>
                <li class="list-group-item">
                    Email : <span class="fw-bold">{{ contact.email }}</span>
                </li>
                <li class="list-group-item">
                    Mobile : <span class="fw-bold">{{ contact.mobile }}</span>
                </li>
                <li class="list-group-item">
                    Company : <span class="fw-bold">{{ contact.company }}</span>
                </li>
                <li class="list-group-item">
                    Title : <span class="fw-bold">{{ contact.title }}</span>
                </li>
                <li class="list-group-item">
                    Group : <span class="fw-bold">{{ group.name }}</span>
                </li>
            </ul>
        </div>
    </div>

    <div class="row mt-3">
        <div class="col">
            <router-link to="/contacts" class="btn btn-success"> <i class="fa fa-arrow-alt-circle-left"></i> Back</router-link>
        </div>
    </div>
</div>
</template>

<script>
import {
    ContactService
} from '../services/ContactService';
import SpinnerComponents from "../components/SpinnerComponents.vue"

export default {
    name: "ViewContact",
    components: {
        SpinnerComponents
    },
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            errorMessage: null,
            group: {}
        }
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContact(this.contactId);
            let groupResponse = await ContactService.getGroup(response.data)
            this.contact = response.data;
            this.group = groupResponse.data;
            this.loading = false;

        } catch (error) {

            this.errorMessage = error;
            this.loading = false;
        }
    }
}
</script>

<style scoped>

</style>
