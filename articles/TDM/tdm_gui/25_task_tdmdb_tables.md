# Task - TDM DB Tables

TDM settings are saved in the [TDM PostgreSQL DB](/articles/TDM/tdm_architecture/02_tdm_database.md). 

TDM tasks update the following TDM DB tables:

## Tasks

This table holds all [TDM tasks](14_task_overview.md) defined in  the TDM GUI.

- A new record is created for each task.

- Each task record gets a unique **task_id** sequence which is the table's PK.

  ### Task General Information

  - **task_title**  - the task name. To prevent creating several active tasks with the same name, the **task_title** column has a **unique index** when the status is **Active**.
  - **task_type** - **Extract** or **Load**.
  - **be_id** - the task's BE. The be_id can be linked to the **product_logical_units** TDM DB table. 
  - **number_of_entities_to_copy** - populated by the [Number of Entities setting](17_load_task_regular_mode.md#number-of-entities) of load tasks.
  - **task_created_by**, and  **task_last_updated_by** - populated by the name of the user who creates the task.
  - **task_creation_date** and **task_last_updated_date** - populated by the task's creation datetime.

  ### Task Status

  - **task_status**: each task is created in **Active** task_status. Deleted tasks have an **Inactive** task_status and are not physically deleted from this table.
  - **task_execution_status**: 
    - **Active** - the task can be executed.
    - **onHold** - [pause the task](/articles/TDM/tdm_gui/26_task_execution.md#holding-task-execution) and set it on-hold.
    - **Inactive** - deleted task.

  ### Requested Entities Columns

  - **selection method**: populated based on the selection method in the [Requested Entities tab](18_load_task_requested_entities_regular_mode.md). This column can be populated by either:
    - **L** - [Entities List](18_load_task_requested_entities_regular_mode.md#entities-list) 
    - **R** - [Random Selection](18_load_task_requested_entities_regular_mode.md#random-selection)
    - **S** - [Create Synthetic Entities](18_load_task_requested_entities_regular_mode.md#create-synthetic-entities).
    - **PR** - [Parameters with a random selection checkbox](18_load_task_requested_entities_regular_mode.md#use-parameters-with-random-selection-checkbox).
    - **P** - [Parameters when a random selection checkbox is cleared](18_load_task_requested_entities_regular_mode.md#use-parameters-with-random-selection-checkbox).
    - **ALL** - select all entities on [Extract](16_extract_task.md#select-all-entities) or [Load Data Flux](20_load_task_dataflux_mode.md#select-all-entities) tasks.
    - **REF** - create a [Reference Only](24_task_reference_tab.md) task.
  - **selection_param_value**: populated when the task selection method is Entities List, Parameters, or Synthetic Data:

  <table width="900pxl">
  <tbody>
  <tr>
  <td width="300pxl">
  <p><strong>Task selection method</strong></p>
  </td>
  <td width="200pxl">
  <p><strong>selection_method</strong></p>
  </td>
  <td width="400pxl">
  <p><strong>Selection_param_value</strong></p>
  </td>
  </tr>
  <tr>
  <td width="300pxl">
  <p>Entities List</p>
  </td>
  <td width="200pxl">
  <p>L</p>
  </td>
  <td width="400pxl">
  <p>Populated by the list of entities separated by a comma.</p>
  <p><strong>Examples</strong>:</p>
  <ul>
  <li>1,2,3,4</li>
  <li>66</li>
  </ul>
  </td>
  </tr>
  <tr>
  <td width="300pxl">
  <p>Parameters, with or without a random selection</p>
  </td>
  <td width="200pxl">
  <p>P, PR</p>
  </td>
  <td width="400pxl">
  <p>Populated by the SQL where statement, generated by the selected parameters.</p>
  <p><strong>Example</strong>:</p>
  <ul>
  <li>(( 'New York' = ANY("Customer.CITY") ))</li>
  </ul>
  </td>
  </tr>
  <tr>
  <td width="300pxl">
  <p>Create Synthetic Entities</p>
  </td>
  <td width="200pxl">
  <p>S</p>
  </td>
  <td width="400pxl">
  <p>Populated by the cloned entity ID.</p>
  <p><strong>Example</strong>:</p>
  <ul>
  <li>7889</li>
  </ul>
  </td>
  </tr>
  </tbody>
  </table>
  <p>&nbsp;</p>

  ​		This column is used by the [TDM task execution process](/articles/TDM/tdm_architecture/03_task_execution_processes.md) to create the entities list of the root LUs on each task.

   

  -  **entity_exclusion_list** - populated by the list of entities separated by a comma, if the [task level exclusion list](18_load_task_requested_entities_regular_mode.md#exclusion-list) is set on load task.

  ### Data Flux Parameters

  - **version_ind** - populated by **true** in a [Data Flux task](15_data_flux_task.md).
  - **selected_version_task_name**, **selected_version_datetime**, and **selected_version_task_exe_id** - the selected entities when creating a load Data Flux task to reload a selected version of entities into the target environment.
  - **selected_ref_version_task_name, selected_ref_version_datetime**, and **selected_ref_version_task_exe_id** - the selected Reference's version when creating a Data Flux task to copy [a selected version of Reference tables](24_task_reference_tab.md) into the target environment.

  ### Environments Columns

  #### Source Environment

- source_env_name 

- source_environment_id

- fabric_environment_name

  #### Target Environment

- environment_id

### Task Execution Parameters

#### Scheduling Parameters

- **Scheduler** - set based on the task's [Execution Timing](22_task_execution_timing_tab.md):
  - **Execution by Request**, populated by **immediate**.
  - **Scheduled Execution**, populated by a **crontab** based on the selected scheduling parameters of the task.
- **scheduling_end_date** - populated by the **End Date** if set on scheduled tasks.

#### Extract Tasks - [Retention Period](16_extract_task.md#retention-period)

- **retention_period_type** and **retention_period_value**.
- Example:
  - Set the retention_period_type by **Days** and the retention_period_value by **5** to set a retention period of five days.

#### Load Tasks - [Request Parameters](19_load_task_request_parameters_regular_mode.md)

##### [Task Operation Mode](19_load_task_request_parameters_regular_mode.md#operation-mode)

The task operation mode is set based on the combination of **load_entity** and **delete_before_load** columns:

<table width="900pxl">
<tbody>
<tr>
<td width="400pxl">
<p><strong>Operation Mode</strong></p>
</td>
<td width="250pxl">
<p><strong>load_entity</strong></p>
</td>
<td width="250pxl">
<p><strong>delete_before_load</strong></p>
</td>
</tr>
<tr>
<td width="400pxl">
<p>Insert Entity without Delete</p>
</td>
<td width="250pxl">
<p>true</p>
</td>
<td width="250pxl">
<p>false</p>
</td>
</tr>
<tr>
<td width="400pxl">
<p>Delete and Load Entity</p>
</td>
<td width="250pxl">
<p>true</p>
</td>
<td width="250pxl">
<p>true</p>
</td>
</tr>
<tr>
<td width="400pxl">
<p>Delete Entity without Load</p>
</td>
<td width="250pxl">
<p>false</p>
</td>
<td width="250pxl">
<p>true</p>
</td>
</tr>
</tbody>
</table>

##### Other Parameters

- **replace_sequences**
- **sync_mode** - 
  - If the task does not override the sync mode, this column remains empty.
  - If the task overrides the [sync mode](19_load_task_request_parameters_regular_mode.md#override-sync-mode), populated by **FORCE** or **OFF**. 

 

## Tasks_Logical_Units

This table holds the LUs list of each task. A separate record is created for each LU.

## Tasks_Post_Exe_Process

-------------------------

This table holds a task's [post execution processes](04_tdm_gui_business_entity_window.md#post-execution-processes-tab). A new record is created for each post execution process.

This table holds the following columns:

-  **task_id** - a unique identifier of the task which links to the **Tasks** TDM DB table.
-  **process_id** - a unique identifier of the process which links [tdm_be_post_exe_process](06_be_product_tdmdb_tables.md#tdm_be_post_exe_process) TDM DB table.
-  **execution_order** - the  execution_order of the post execution process as defined in the [tdm_be_post_exe_process](06_be_product_tdmdb_tables.md#tdm_be_post_exe_process) TDM DB table. 

## Task_Globals

This table holds all [Globals that are overridden by the task](23_task_globals_tab.md). A separate record is created for each Global.

## Task_Ref_Tables

This table holds a list of the task's [Reference tables](24_task_reference_tab.md). A separate record is created for each Reference table.

  [![Previous](/articles/images/Previous.png)](24_task_reference_tab.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](26_task_execution.md)



