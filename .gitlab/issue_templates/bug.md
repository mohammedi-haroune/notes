<!--These are comments and will not be rendered. Please click Preview to check.-->


### Summary:
------------

<!--Briefly summarize the problem. What, where, when, who, why, and how.-->


### Reproduce:
--------------

<!-- What are the clear steps to reproduce the bug? For example:

- As a logged in user, visit <route>
- Click on button <y>
- Select icon <z>
-->

### Actual outcome:
-------------------

<!--What was the outcome of these actions and steps?-->


### Expected outcome:
---------------------

<!--What did you expect the outcome to be, if different?-->


### Logs &/or traceback:
--------------------------

<details>
<summary>Traceback</summary>
<pre>

Nasty exception

</pre>
</details>



<details>
<summary>Log</summary>
<pre>

2018-01-17 11:09:33,258 INFO sqlalchemy.engine.base.Engine SELECT users_have_groups.id AS users_have_groups_id, 

</pre>
</details>




### Likely parts and draft fix:
----------------------------------

<!--

Where do you think we can act in the code to fix the bug

File: `foo/example.py`

```python
import this
```

Change to:

```python
import antigravity
```

-->

### Check list
----------------------------------
<!--

Reminders to important things to check before closing the issue

-->
- [ ] Write unit tests that fail before the fix and pass after it.



/label ~bug
/cc @rahimbk @jhadjar @hadjkhelil
