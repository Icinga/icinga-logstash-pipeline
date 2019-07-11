# Naming scheme
This document will provide basic informations the naming scheme used within this project. It is essential to read, before contributing.

The filters try to adhere to the [ECS](https://www.elastic.co/guide/en/ecs/1.0/index.html) field named. Those fields which are not covered by the ECS are described below. Please keep in mind that it's your responsibility to set the correct types of the fields if you want to use ECS. Most of them should be detected automatically in the right way, though.

### Basics
The basics of the naming scheme are pretty easy and should be comprehended after taking a first glance at one of the config files.

The **id** should be like this *icinga_NAMEOFLOGMESSAGE*, aswell as **add_tag**.
The **add_field** section with **[icinga][eventtype]** always equals the name provided in the **id**, with _ to help with the readability.
Every **grok filter** will get the following **tags_on_failure** _grokparsefailure and icinga_NAMEOFLOGMESSAGE_failed.

Example:
```sh
id => "icinga_NAMEOFLOGMESSAGE"
add_tag => "icinga_NAMEOFLOGMESSAGE"
tag_on_failure => ["_grokparsefailure","icinga_NAMEOFLOGMESSAGE_failed"]
add field => {
  "[icinga][eventtype]" => "NAME_OF_LOG_MESSAGE"
}
```

### Fieldnames

The basic naming scheme is **[icinga]** + a constructed field name based on the *use* of the field and *context*. Below are examples for fieldnames by use, all of these are currently used and you are encouraged to use those. More fieldnames will make the naming scheme more and more complex.

#### Fieldnames by use

Examples for fieldnames by use:
- [object]
- [bytes]
- [count]
- [type]
- [current]
- [zone]
- [detail]
- [message]
- [position]
- [master]
- [endpoint]
- [endpoints]
- [notification]
- [check]
- [type]
- [state]

#### Fieldnames by context

Often you will run into circumstances where the content of a field won’t be sufficiently explained by the fieldname given, If you only use fieldnames by *use*. Then you'll have to add a prefix or suffix based on the context of the field. Of course only add a context-prefix or -suffix if necessary. Please, don’t separate neither words, suffixes nor prefixes with _.

#### Constructed fieldnames

Examples for prefix and suffix build fieldnames:
[icinga][clients] → [icinga][remainingclients]
[icinga][position] → [icinga][logposition]
[icinga][endpoints] → [icinga][connectedendpoints]
[icinga][host] → [icinga] [listenerhost]
[icinga][host] → [icinga] [clienthost]

Sometimes additional fieldnames by use provide the context needed:
[icinga][file] → [icinga][statefile]
[icinga][type] → [icinga][objecttype]

At the end of this documentation is a list of all subfields of [icinga] used until now. If at all possible, try to check if a field you want to use has been used before or check if similar fields to yours are used already and adjust accordingly to prevent confusing new fields. Before inventing new fields, please check the [ECS field reference](https://www.elastic.co/guide/en/ecs/1.0/index.html) if a fitting field is already defined.

**Exception** Of course there are also unique fields, which are sadly necessary but should be avoided.

### Used fieldnames

When contributing to this project, run this command and replace the fieldnames used with its output. 

```sh
grep -Pho "\[icinga\]\[[^\[]*?\]" filter-* | sort -u | sed -e "s@\[icinga\]\[@@;s@\]@,@" | sed ':a;N;$!ba;s/\n/ /g' | sed -e "s/,$//"
```

These are all fieldnames in use for filter-50-configs to date:

*apirequest, bytes, checkablespending, checkablesrate, checkinterval, checknext, checkoriginal, checktime, clientendpoint, code, command, component, configfilecount, connectedendpoints, context, count, currentepoch, currentmaster, date, dateend, datestart, dbinstance, detail, direction, endtime, epochcurrent, epochreceived, eventtype, exitcode, facility, filecount, filterversion, fstate, ftype, idlecheckables, items01min, items05min, items15min, itemscount, itemsrate, logposition, message, messagecount, messagetype, metriclist, name, nomessageduration, notification, notificationcount, notificationtype, object, objectdetails, objectname, objecttype, period, pluginarguments, pluginexitcode, pluginoutput, query, receivedepoch, remainingclients, severity, signalcode, signaldetail, sslerrorcode, sslerrordetails, starttime, state, statefile, statefilter, statefilterid, stride, timerange, timestamp, typefilter, typefilterid, weekday, workerdetail, workerfacility, workerid, zone*

### Arrays

We don't want to use arrays at all. Not just try to avoid them, please avoid them. We intend to use different fields for different data, which is key to logmanagement. If you really have to you can use yet another nested field *[icinga][nested][field]*, but even then the content should be related enough to combine them afterwards. This way we don't get an array, but string. The optimal solution of course is to use a [@metadata] field. 

### Contribution-Dashboard

In this repository among other useful dashboards is a contribution.json, which provides a basic dashboard useful to identify areas which are in need of the most contribution. It gives you a nice overview of _grokparsefailures and undefined icinga.facility fields.
