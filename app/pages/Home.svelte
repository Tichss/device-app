<script lang="ts">
    import * as appSettings from "@nativescript/core/application-settings";
    import { onMount } from "svelte";
    import { navigate } from "svelte-native";
    import EditDevice from "./EditDevice.svelte";
    import Device from "../components/Device.svelte";
    import { devices } from "../store";
    import { requestPermission } from "nativescript-permissions";

    onMount(() => {
        $devices = JSON.parse(appSettings.getString("devices", "[]"));
        console.log($devices);

        requestPermission(
            android.Manifest.permission.CALL_PHONE,
            "I need these permissions because I'm cool"
        )
            .then(() => {
                console.log("Woo Hoo, I have the power!");
            })
            .catch(() => {
                console.log("Uh oh, no permissions - plan B time!");
            });
    });

    function onNewDevice() {
        navigate({
            page: EditDevice,
            props: { device: { name: "", phone: "", smses: [] }, isNew: true },
        });
    }
</script>

<page>
    <actionBar title="Eszközök">
        <actionItem
            ios.systemIcon="16"
            ios.position="right"
            text="Eszköz hozzáadása"
            android.position="popup"
        />
    </actionBar>

    <scrollView>
        <wrapLayout>
            <button text="add" on:tap={onNewDevice} />
            {#each $devices as device, i}
                <Device {device} {i} />
            {/each}
        </wrapLayout>
    </scrollView>
</page>

<style lang="scss">
    button {
        height: 90;
        width: 90;
        margin: 5;
        border-radius: 20;
    }
    wrapLayout {
        horizontal-alignment: center;
        background-color: rebeccapurple;
    }
</style>
