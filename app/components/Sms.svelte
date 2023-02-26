<script context="module" lang="ts">
    export type Sms = {
        name: string;
        text: string;
    };
</script>

<script lang="ts">
    import { navigate } from "svelte-native";
    import Device from "./Device.svelte";
    import EditSms from "../pages/EditSms.svelte";

    export let parent: Device;
    export let sms: Sms;

    function onEdit() {
        console.log("parent", parent);
        console.log("sms", sms);

        navigate({
            page: EditSms,
            props: { parent, sms },
        });
    }
</script>

<flexboxLayout justifyContent="space-between">
    <stackLayout>
        <label class="sms-name" text={sms.name} />
        <label class="sms-text" text={sms.text} />
    </stackLayout>
    <stackLayout orientation="horizontal">
        <image src="~/images/send.png" on:tap={onEdit} />
        <image src="~/images/test.png" on:tap={onEdit} />
    </stackLayout>
</flexboxLayout>

<style lang="scss">
    flexboxLayout {
        padding: 20px;
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;

        stackLayout {
            .sms-name {
                font-size: 20px;
                margin-top: -20px;
                margin-bottom: -20px;
            }

            .sms-text {
                font-size: 15px;
                margin-bottom: -10px;
            }
        }

        image {
            height: 20;
        }

        button {
            background-color: red;
        }
    }
</style>
