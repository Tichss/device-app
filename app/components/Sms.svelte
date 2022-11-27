<script context="module" lang="ts">
    export type Sms = {
        name: string;
        text: string;
    };
</script>

<script lang="ts">
    export let sms: Sms;

    function onEdit() {
        console.log("asd");
    }
</script>

<flexboxLayout justifyContent="space-between" on:tap={onCall}>
    <label text={sms.name} />
    <button text="Edit" on:tap={onEdit} />
</flexboxLayout>

<style lang="scss">
    flexLayout {
        background-color: blue;
    }
</style>
