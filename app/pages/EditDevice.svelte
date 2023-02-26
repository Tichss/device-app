<script lang="ts">
    import { navigate } from "svelte-native";
    import { devices } from "../store";
    import { setString } from "@nativescript/core/application-settings";
    import { goBack } from "svelte-native";
    import type { Device } from "~/components/Device.svelte";
    import { confirm } from "@nativescript/core/ui/dialogs";
    import Sms from "~/components/Sms.svelte";
    import EditSms from "../pages/EditSms.svelte";
    
    export let device: Device = { name: "", phone: "", smses: [] };
    export let isNew = false;

    $: device, console.log('device', device);
    

    function onSave() {
        if (isNew) {
            $devices.push(device);
            setString("devices", JSON.stringify($devices));
        }
        $devices = $devices;
        goBack();
    }

    function onDelete() {
        confirm({
            title: "Törlés",
            message: `Törlöd az ezközt: ${device.name}?`,
            okButtonText: "Törlés",
            cancelButtonText: "Mégse",
        }).then((res) => {
            if (res) {
                $devices = $devices.filter((dev) => dev.name !== device.name);
                setString("devices", JSON.stringify($devices));
                goBack();
            }
        });
    }

    function onAddSms() {
        navigate({
            page: EditSms,
            props: { parentDevice: device, sms: { name: "", text: "" }, isNew: true },
        });
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
                on:tap={onDelete}
            />
        {/if}
    </actionBar>
    <scrollView>
        <stackLayout>
            <label text="Eszközneve*" />
            <textField bind:text={device.name} />
            <label text="Eszköz száma*" />
            <textField bind:text={device.phone} keyboardType="number" />
            <label class="sms-label" text="Eszköz sms listája" />
            <label text={device.smses.length.toString()} />
            <stackLayout>
                {#each device.smses as sms}
                    <label text="t1" />
                    <Sms {sms} parent={device} />
                {/each}
            </stackLayout>
            <button text="Sms hozzáadása" on:tap={onAddSms} />
            <button
                text="Mentés"
                on:tap={onSave}
                isEnabled={!!device.name && !!device.phone}
            />
            {#if !isNew}
                <button text="Törlés" on:tap={onDelete} />
            {/if}
        </stackLayout>
    </scrollView>
</page>

<style lang="scss" scoped>
    stackLayout {
        padding: 30px;

        label {
            font-size: 16px;
            color: gray;
        }

        textField {
            margin: 0;
            font-size: 18px;
        }

        .sms-label {
            margin-top: 50px;
        }
    }
</style>
