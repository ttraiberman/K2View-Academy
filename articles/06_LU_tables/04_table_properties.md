# LU Table Properties

The Table Properties tab is displayed in the right pane of the Table's window.


![image](images/06_04_table_properties.png)


The Properties tab displays a list of properties that must be defined for each LU table, as follows:

<table width="623">
<tbody>
<tr>
<td width="150pxl">
<p><strong>Main</strong></p>
</td>
<td width="700pxl">
<p>Non-editable fields:</p>
<ul>
<li>Name, LU table name.</li>
<li>ID, generated by Fabric.</li>
</ul>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Instance PK Column</strong></p>
</td>
<td width="474">
<p>A unique field that is used as the <a href="/articles/06_LU_tables/03_table_indexes.md#index-definition">LU table&rsquo;s Primary Key.</a></p>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Columns Collation</strong></p>
</td>
<td width="474">
<p>There are three options:</p>
<ul>
<li>BINARY, (default) compares the exact string in the field with the SQL statement.</li>
<li>NOCASE, enables the Select statement to ignore upper / lower case fonts when comparing text fields. For example: <br /> Select TYPE from tblExample where NAME = &lsquo;value&rsquo; returns records when the NAME field is set either to &lsquo;VALUE&rsquo; or &lsquo;value&rsquo;.</li>
<li>RTRIM, enables the Select statement to ignore white space characters on the right of the string when comparing text fields. For example:<br /> Select TYPE from tblExample where the NAME = &lsquo;value&rsquo; returns records that match both &lsquo;value&rsquo; and &lsquo;value &lsquo;.</li>
</ul>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Full Text Search</strong></p>
</td>
<td width="474">
<p>When set to True, enables the use of the MATCH Sqlite command as part of the WHERE clause of a Select statement that reads data from a Fabric table. Default = False.</p>
<p>Click for more information about the Match command:</p>
<p><a href="http://www.sqlite.org/fts3.html#section_3">http://www.sqlite.org/fts3.html#section_3</a></p>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Sync Method</strong></p>
</td>
<td width="474">
<p>There are four <a href="/articles/14_sync_LU_instance/04_sync_methods.md">Sync methods</a>:</p>
<ul>
<li>None.</li>
<li>Inherited (default).</li>
<li>Time Interval.</li>
<li>Decision Function.</li>
</ul>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Truncate Before Sync</strong></p>
</td>
<td width="474">
<p>When <a href="/articles/14_sync_LU_instance/04_sync_methods.md#truncate-before-sync">Truncate Before Sync</a> = True, the entire LU table is truncated before the populations are executed.</p>
</td>
</tr>
<tr>
<td width="150">
<p><strong>Enrichment Functions</strong></p>
</td>
<td width="474">
<p><a href="/articles/10_enrichment_function/01_enrichment_function_overview.md">Enrichment Functions</a> which are executed after all LU tables are populated.</p>
<ul>
<li>The execution order is determined on an LU level and based on the Sync policy of the attached table. When no Enrichment function is attached - displays &lsquo;Empty&rsquo;.</li>
<li>When one or more Enrichment functions are attached &ndash; displays &lsquo;&lt;x&gt; enrichments&rsquo; (where &lt;x&gt; is the number of attached Enrichment functions).</li>
</ul>
<p>To select an Enrichment function, click the three dots next to the Enrichment functions property and select the function name. Only functions without input and output parameters are displayed.</p>
</td>
</tr>
</tr>
<tr>
<td width="150">
<p><strong>On Change</strong></p>
</td>
<td width="474">
<p><a href="/articles/07_table_population/11_4_creating_a_trigger_function.md">Trigger functions</a> which are executed when there is a change in LU table's data.</p>
<p>To select a Trigger function, click the three dots next to the On Change property and select the function name. Only Trigger functions are displayed.</p>
</td>
</tr>
</tbody>
</table>

[![Previous](/articles/images/Previous.png)](/articles/06_LU_tables/03_table_indexes.md)