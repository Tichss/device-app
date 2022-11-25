<script context="module" lang="ts">
    export type Device = {
        name: string;
        phone: string;
        smses: Array<string>;
    };
</script>

<script lang="ts">
    import { navigate } from "svelte-native";
    import EditDevice from "../pages/EditDevice.svelte";
    import { goBack } from "svelte-native";
    import { dial } from "nativescript-phone";

    export let device: Device;
    export let i: number;
    let stop = true;

    function onEdit() {
        console.log("onEdit");
        navigate({
            page: EditDevice,
            props: { device },
        });
        stop = false;
    }

    function onCall() {
        /* setTimeout(() => {
            if (stop) {
                console.log("onCall");
                dial(device.phone, false);
            }
        }, 100);
        stop = true; */
    }
</script>

<flexboxLayout on:tap={onCall}>
    <label text={device.name} />
    <button text="Edit Device" on:tap={onEdit} />
</flexboxLayout>

<style lang="scss">
    button {
        margin: 0;
    }

    flexboxLayout {
        margin: 5;
        width: 90;
        height: 90;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        background-color: #3c495e;
        border-radius: 15;
    }
</style>
