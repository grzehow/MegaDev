<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';
  import deskNotes from '#lib/data/desk-notes.json';

  const merged = uniqueById(reports as Report[]).filter((r) => r.mergedInto === 4471);
  const oneDayIds = new Set(deskNotes.oneDayMove);
  const oneDay = merged.filter((r) => oneDayIds.has(r.id));
  const parser = merged.filter((r) => !oneDayIds.has(r.id) && causeOf(r, tenantOf(r)) === 'parser');
  const hint = merged.filter((r) => !oneDayIds.has(r.id) && causeOf(r, tenantOf(r)) === 'hint');
  const unplaced = merged.length - parser.length - hint.length - oneDay.length;
</script>

<table>
  <thead><tr><th>Problem</th><th>Reports</th></tr></thead>
  <tbody>
    <tr><td>Month-first parser for day-first tenants</td><td>{parser.length}</td></tr>
    <tr><td>Month-first hint on day-first booking-v3 forms</td><td>{hint.length}</td></tr>
    <tr><td>One-day moves (symptom under investigation)</td><td>{oneDay.length}</td></tr>
    {#if unplaced}<tr><td>Unplaced; investigate</td><td>{unplaced}</td></tr>{/if}
    <tr><th>Total</th><th>{merged.length}</th></tr>
  </tbody>
</table>
