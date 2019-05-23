# Naming scheme
This document will provide basic informations the naming scheme used within this project. It is essential to read, before contributing.

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
- [client]
- [clients]
- [host]
- [hosts]
- [port]
- [ports]
- [bytes]
- [count]
- [name]
- [type]
- [current]
- [zone]
- [detail]
- [message]
- [position]
- [master]
- [endpoint]
- [endpoints]
- [path]
- [notification]
- [check]
- [file]
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

At the end of this documentation is a list of all subfields of [icinga] used until now. If at all possible, try to check if a field you want to use has been used before or check if similar fields to yours are used already and adjust accordingly to prevent confusing new fields.

**Exception** Of course there are also unique fields, which are sadly necessary but should be avoided.

### Used fieldnames

These are all fieldnames in use for filter-50-configs to date:

*port, bytes, messagecount, objectname, objecttype, filecount, epohcurrent, clientendport, direction, clienthost, clientport, zone, detail, remainingclients, message, logposition, currentmaster, connectedendpoints, path, listenerhost, notificationcount, checkoriginal, checknext, checkinterval, changetype, stateoriginal, statenew, idlecheckables, checkablesrate, checktime, statefile, nomessageduration, messagetype, clientendpoint, dbinstanceid, dbinstancescheme, notificationtype, username, notification, pluginpid, plugin, pluginarguments, pluginexitcode, pluginoutput, pid, signalcode, signaldetail, itemscount, itemsrate, items01min, items05min, items15min, sslerrordetails*


