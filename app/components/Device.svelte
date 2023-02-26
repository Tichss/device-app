<script context="module" lang="ts">
    import type { Sms } from "~/components/Sms.svelte";

    export type Device = {
        name: string;
        phone: string;
        smses: Array<Sms>;
    };
</script>

<script lang="ts">
    import { navigate } from "svelte-native";
    import EditDevice from "../pages/EditDevice.svelte";
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
        console.log("onCall");

        setTimeout(() => {
            if (stop) {
                /* dial(device.phone, false); */
            }
        }, 100);
        stop = true;
    }
</script>

<absoluteLayout width="100%" height="100%">
    <image src="~/images/test.png" on:tap={onEdit} />
    <flexboxLayout>
        <image src="~/images/device.png" on:tap={onCall} />
        <label class="name" text={device.name} />
        <label class="phone" text={device.phone} />
    </flexboxLayout>
</absoluteLayout>

<style lang="scss">
    absoluteLayout {
        height: 100%;
        width: 100%;

        image {
            left: 5;
            top: 5;
        }

        flexboxLayout {
            height: 100%;
            width: 100%;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: black;

            .name {
                margin-bottom: -6;
            }

            .phone {
                color: gray;
                font-size: 10;
            }

            image {
                height: 40;
            }
        }
    }
</style>
