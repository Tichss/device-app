<script lang="ts">
    import { navigate } from "svelte-native";
    import Home from "./Home.svelte";
    import { devices } from "../store";
    import { setString } from "@nativescript/core/application-settings";
    import { goBack } from "svelte-native";
    import type { Device } from "~/components/Device.svelte";
    import { confirm } from "@nativescript/core/ui/dialogs";
    import Sms from "~/components/Sms.svelte";

    export let device: Device = { name: "", phone: "", smses: [] };
    export let isNew = false;

    function onSave() {
        if (isNew) {
            $devices.push(device);
            $devices = $devices;
            setString("devices", JSON.stringify($devices));
        }
        goBack();
    }

    function onDelete() {
        confirm({
            title: "Törlés",
            message: `Törlöd az ezközt: ${device.name}?`,
            okButtonText: "Törlés",
            cancelButtonText: "Mégse",
        }).then((res) => {
            console.log(res);
            if (res) {
                $devices = $devices.filter((dev) => dev.name !== device.name);
                setString("devices", JSON.stringify($devices));
                goBack();
            }
        });
    }

    function onAddSms() {
        device.smses.push({ name: "Test1", text: "test1" });
        device.smses = device.smses;
        console.log("LEFUTOTT");
    }
</script>

<page>
    <actionBar title={isNew ? "Új eszköz" : `Eszköz: ${device.name}`}>
        {#if !isNew}
            <actionItem
                ios.systemIcon="16"
                ios.position="right"
                text="Törlés"
                android.position="popup"
            />
        {/if}
    </actionBar>
    <scrollView>
        <stackLayout>
            <label text="Eszközneve*" />
            <textField bind:text={device.name} />
            <label text="Eszköz száma*" />
            <textField bind:text={device.phone} keyboardType="number" />
            <label text="Eszköz sms listája" />
            {#if device.smses?.length > 0}
                {#each device.smses as sms}
                    <Sms {sms} />
                {/each}
            {/if}
            <!-- {#if device.smses?.length > 0}
                {#each device.smses as sms}
                    <textField bind:text={sms} />
                {/each}
            {/if} -->
            <button text="Sms hozzáadása" on:tap={onAddSms} />
            <button text="Mentés" on:tap={onSave} />
            {#if !isNew}
                <button text="Törlés" on:tap={onDelete} />
            {/if}
        </stackLayout>
    </scrollView>
</page>

<style lang="scss">
    /* your styles go here */
</style>
