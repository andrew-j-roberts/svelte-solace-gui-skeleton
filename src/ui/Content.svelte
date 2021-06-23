<script>
  import { getContext } from "svelte";
  import { solaceContextKey } from "../solace/client";

  const { getSolaceClient } = getContext(solaceContextKey);
  let solaceClient = getSolaceClient();

  let data_tableA = {
    key_A: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_B: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_C: { property_A: "foo", property_B: "bar", property_C: "baz" },
  };

  let data_tableB = {
    key_A: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_B: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_C: { property_A: "foo", property_B: "bar", property_C: "baz" },
  };

  let data_tableC = {
    key_A: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_B: { property_A: "foo", property_B: "bar", property_C: "baz" },
    key_C: { property_A: "foo", property_B: "bar", property_C: "baz" },
  };

  const handler_eventA = (message) => updateData_tableA(parse(message));
  const handler_eventB = (message) => updateData_tableB(parse(message));
  const handler_eventC = (message) => updateData_tableC(parse(message));

  function updateData_tableA(eventObj) {
    // this mock example would update eventObj's key to the new eventObj value
    data_tableA = { ...data_tableA, [eventObj[id]]: eventObj };
  }

  function updateData_tableB(eventObj) {
    // this mock example would update eventObj's key to the new eventObj value
    data_tableA = { ...data_tableA, [eventObj[id]]: eventObj };
  }

  function updateData_tableC(eventObj) {
    // this mock example would update eventObj's key to the new eventObj value
    data_tableA = { ...data_tableA, [eventObj[id]]: eventObj };
  }

  function parse(message) {
    let container = message.getSdtContainer();
    if (container != null) {
      return container.getValue();
    } else {
      return JSON.parse(message.getBinaryAttachment());
    }
  }

  // runs when either  solaceClient or exchangeFeedStates updates
  $: if ($solaceClient) {
    $solaceClient.subscribe(`topicA`, handler_eventA);
    $solaceClient.subscribe(`topicB`, handler_eventB);
    $solaceClient.subscribe(`topicC`, handler_eventC);
  }
</script>

<div class="w-full h-full p-4">
  <!-- top two tables -->
  <div class="grid grid-cols-2 gap-4">
    <!-- table A -->
    <table>
      <thead>
        <tr>
          <th>property_A</th>
          <th>property_B</th>
          <th>property_C</th>
        </tr>
      </thead>
      <tbody>
        {#each Object.keys(data_tableA) as key}
          <tr>
            <td>
              {data_tableA[key]["property_A"]}
            </td>
            <td>
              {data_tableA[key]["property_B"]}
            </td>
            <td>
              {data_tableA[key]["property_C"]}
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
    <!-- table B -->
    <table>
      <thead>
        <tr>
          <th>property_A</th>
          <th>property_B</th>
          <th>property_C</th>
        </tr>
      </thead>
      <tbody>
        {#each Object.keys(data_tableB) as key}
          <tr>
            <td>
              {data_tableA[key]["property_A"]}
            </td>
            <td>
              {data_tableA[key]["property_B"]}
            </td>
            <td>
              {data_tableA[key]["property_C"]}
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  <!-- bottom tables -->
  <div class="mt-4">
    <table>
      <thead>
        <tr>
          <th>property_A</th>
          <th>property_B</th>
          <th>property_C</th>
        </tr>
      </thead>
      <tbody>
        {#each Object.keys(data_tableC) as key}
          <tr>
            <td>
              {data_tableA[key]["property_A"]}
            </td>
            <td>
              {data_tableA[key]["property_B"]}
            </td>
            <td>
              {data_tableA[key]["property_C"]}
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>

<style>
  table {
    border-collapse: collapse;
    width: 100%;
  }

  td,
  th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>
