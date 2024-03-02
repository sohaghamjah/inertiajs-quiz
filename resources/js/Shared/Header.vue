<script setup>
    import { Link } from '@inertiajs/vue3';
</script>
<template>
    <header class="d-flex justify-content-center py-3">
        <ul class="nav nav-pills">
            <li class="nav-item">
                <Link href="#" class="nav-link active" aria-current="page">Home</Link>
            </li>
            <li class="nav-item">
                <Link href="#" class="nav-link">Features</Link>
            </li>
            <li class="nav-item">
                <Link href="#" class="nav-link">Pricing</Link>
            </li>
            <li class="nav-item">
                <Link href="#" class="nav-link">FAQs</Link>
            </li>
            <li class="nav-item">
                <Link href="#" class="nav-link">About</Link>
            </li>
        </ul>
    </header>
</template>