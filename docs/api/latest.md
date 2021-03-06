# API Docs - v4.0.11

## Akslack

### reorder *<a target="_blank" href="https://wso2.github.io/siddhi/documentation/siddhi-4.0/#stream-processor">(Stream Processor)</a>*

<p style="word-wrap: break-word">This stream processor extension performs reordering of an out-of-order event stream.<br>&nbsp;It implements the AQ-K-Slack based out-of-order handling algorithm (originally described in <br>http://dl.acm.org/citation.cfm?doid=2675743.2771828)</p>

<span id="syntax" class="md-typeset" style="display: block; font-weight: bold;">Syntax</span>
```
akslack:reorder(<LONG> timestamp, <DOUBLE> correlation.field, <LONG> batch.size, <LONG> timer.timeout, <LONG> max.k, <BOOL> discard.flag, <DOUBLE> error.threshold, <DOUBLE> confidence.level)
```

<span id="query-parameters" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">QUERY PARAMETERS</span>
<table>
    <tr>
        <th>Name</th>
        <th style="min-width: 20em">Description</th>
        <th>Default Value</th>
        <th>Possible Data Types</th>
        <th>Optional</th>
        <th>Dynamic</th>
    </tr>
    <tr>
        <td style="vertical-align: top">timestamp</td>
        <td style="vertical-align: top; word-wrap: break-word">Attribute used for ordering the events</td>
        <td style="vertical-align: top"></td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">No</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">correlation.field</td>
        <td style="vertical-align: top; word-wrap: break-word">Corresponds to the data field of which the accuracy directly gets affected by the adaptive operation of the Alpha K-Slack extension. This field is used by the Alpha K-Slack to calculate the runtime window coverage threshold which is an upper limit set for the unsuccessfully handled late arrivals</td>
        <td style="vertical-align: top"></td>
        <td style="vertical-align: top">DOUBLE</td>
        <td style="vertical-align: top">No</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">batch.size</td>
        <td style="vertical-align: top; word-wrap: break-word">The parameter batch.size denotes the number of events that should be considered in the calculation of an alpha value. batch.size should be a value which should be greater than or equals to 15</td>
        <td style="vertical-align: top">10,000</td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">timer.timeout</td>
        <td style="vertical-align: top; word-wrap: break-word">Corresponds to a fixed time-out value in milliseconds, which is set at the beginning of the process. Once the time-out value expires, the extension drains all the events that are buffered within the reorder extension to outside. The time out has been implemented internally using a timer. The events buffered within the extension are released each time the timer ticks.</td>
        <td style="vertical-align: top">-1 (timeout is infinite)</td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">max.k</td>
        <td style="vertical-align: top; word-wrap: break-word">The maximum threshold value for K parameter in the Alpha K-Slack algorithm</td>
        <td style="vertical-align: top">9,223,372,036,854,775,807 (The maximum Long value)</td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">discard.flag</td>
        <td style="vertical-align: top; word-wrap: break-word">Indicates whether the out-of-order events which appear after the expiration of the Alpha K-slack window should get discarded or not. When this value is set to true, the events would get discarded</td>
        <td style="vertical-align: top">false</td>
        <td style="vertical-align: top">BOOL</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">error.threshold</td>
        <td style="vertical-align: top; word-wrap: break-word">Error threshold to be applied in Alpha K-Slack algorithm. This parameter must be defined simultaneously with confidenceLevel</td>
        <td style="vertical-align: top">0.03 (3%)</td>
        <td style="vertical-align: top">DOUBLE</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">confidence.level</td>
        <td style="vertical-align: top; word-wrap: break-word">Confidence level to be applied in Alpha K-Slack algorithm. This parameter must be defined simultaneously with errorThreshold</td>
        <td style="vertical-align: top">0.95 (95%)</td>
        <td style="vertical-align: top">DOUBLE</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
</table>

<span id="examples" class="md-typeset" style="display: block; font-weight: bold;">Examples</span>
<span id="example-1" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 1</span>
```
define stream inputStream (eventtt long,data double);
@info(name = 'query1')
from inputStream#reorder:akslack(eventtt, data, 20)
select eventtt, data
insert into outputStream;
```
<p style="word-wrap: break-word">This query performs reordering based on the 'eventtt' attribute values. In this example, 20 represents the batch size</p>

## Kslack

### reorder *<a target="_blank" href="https://wso2.github.io/siddhi/documentation/siddhi-4.0/#stream-processor">(Stream Processor)</a>*

<p style="word-wrap: break-word">This stream processor extension performs reordering of an out-of-order event stream.<br>&nbsp;It implements the K-Slack based out-of-order handling algorithm (originally described in <br>https://www2.informatik.uni-erlangen.de/publication/download/IPDPS2013.pdf)</p>

<span id="syntax" class="md-typeset" style="display: block; font-weight: bold;">Syntax</span>
```
kslack:reorder(<LONG> timestamp, <LONG> timer.timeout, <LONG> max.k, <BOOL> discard.flag)
```

<span id="query-parameters" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">QUERY PARAMETERS</span>
<table>
    <tr>
        <th>Name</th>
        <th style="min-width: 20em">Description</th>
        <th>Default Value</th>
        <th>Possible Data Types</th>
        <th>Optional</th>
        <th>Dynamic</th>
    </tr>
    <tr>
        <td style="vertical-align: top">timestamp</td>
        <td style="vertical-align: top; word-wrap: break-word">Attribute used for ordering the events</td>
        <td style="vertical-align: top"></td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">No</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">timer.timeout</td>
        <td style="vertical-align: top; word-wrap: break-word">Corresponds to a fixed time-out value in milliseconds, which is set at the beginning of the process. Once the time-out value expires, the extension drains all the events that are buffered within the reorder extension to outside. The time out has been implemented internally using a timer. The events buffered within the extension are released each time the timer ticks.</td>
        <td style="vertical-align: top">-1 (timeout is infinite)</td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">max.k</td>
        <td style="vertical-align: top; word-wrap: break-word">The maximum threshold value for K parameter in the K-Slack algorithm</td>
        <td style="vertical-align: top">9,223,372,036,854,775,807 (The maximum Long value)</td>
        <td style="vertical-align: top">LONG</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
    <tr>
        <td style="vertical-align: top">discard.flag</td>
        <td style="vertical-align: top; word-wrap: break-word">Indicates whether the out-of-order events which appear after the expiration of the K-slack window should get discarded or not. When this value is set to true, the events would get discarded</td>
        <td style="vertical-align: top">false</td>
        <td style="vertical-align: top">BOOL</td>
        <td style="vertical-align: top">Yes</td>
        <td style="vertical-align: top">No</td>
    </tr>
</table>

<span id="examples" class="md-typeset" style="display: block; font-weight: bold;">Examples</span>
<span id="example-1" class="md-typeset" style="display: block; color: rgba(0, 0, 0, 0.54); font-size: 12.8px; font-weight: bold;">EXAMPLE 1</span>
```
define stream inputStream (eventtt long, price long, volume long);
@info(name = 'query1')
from inputStream#reorder:kslack(eventtt, 1000)
select eventtt, price, volume
insert into outputStream;
```
<p style="word-wrap: break-word">This query performs reordering based on the 'eventtt' attribute values. In this example, the timeout value is set to 1000 milliseconds</p>

