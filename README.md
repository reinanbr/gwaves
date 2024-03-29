<div align='center'>
 <h1>gwaves</h1>
<p>A python lib for works with gravitational waves data</p>
<img alt="CodeFactor Grade" src="https://img.shields.io/codefactor/grade/github/reinanbr/gwaves?logo=codefactor"><img alt="Code Climate maintainability" src="https://img.shields.io/codeclimate/maintainability-percentage/reinanbr/dreams"><img alt="GitHub Pipenv locked Python version" src="https://img.shields.io/github/pipenv/locked/python-version/reinanbr/gwaves">
<br/>
<a href='https://pypi.org/project/dreams/'><img src='https://img.shields.io/pypi/v/gwaves'></a><img alt="PyPI - License" src="https://img.shields.io/pypi/l/gwaves?color=b"><a href="https://doi.org/10.5281/zenodo.7966690"><img src="https://zenodo.org/badge/DOI/10.5281/zenodo.7966690.svg" alt="DOI"></a>

<hr>



</div>

## installing:

```sh
pip3 install gwaves -U
```

## importing
```py
import gwaves as gw
```
## geting the all data:
```py
gw_data = gw.Gwaves_Data()

gw_data.data_gw

```
result:
```sh
                                                                                                                             (base) 
    Unnamed: 0             Name Version           Release  ...          Pastro          Final Mass (M☉)                     Date                                               Link
0            0  GW200322_091133      v1  GWTC-3-confident  ...  0.077572  0.08       53.0  53  +38  -26  2020-03-22T09:12:10.300  https://www.gw-openscience.org/eventapi/html/G...
1            1  GW200316_215756      v1  GWTC-3-confident  ...   0.99  ≥  0.99   20.2  20.2  +7.4  -1.9  2020-03-16T21:58:33.100  https://www.gw-openscience.org/eventapi/html/G...
2            2  GW200311_115853      v1  GWTC-3-confident  ...   0.99  ≥  0.99   59.0  59.0  +4.8  -3.9  2020-03-11T11:59:30.300  https://www.gw-openscience.org/eventapi/html/G...
3            3  GW200308_173609      v1  GWTC-3-confident  ...    0.8566  0.86  47.4  47.4  +11.1  -7.7  2020-03-08T17:36:46.700  https://www.gw-openscience.org/eventapi/html/G...
4            4  GW200306_093714      v1  GWTC-3-confident  ...   0.24004  0.24  41.7  41.7  +12.3  -6.9  2020-03-06T09:37:51.100  https://www.gw-openscience.org/eventapi/html/G...
..         ...              ...     ...               ...  ...             ...                      ...                      ...                                                ...
88          88         GW170608      v3  GWTC-1-confident  ...       1.0  1.00   17.8  17.8  +3.4  -0.7  2017-06-08T02:01:53.500  https://www.gw-openscience.org/eventapi/html/G...
89          89         GW170104      v2  GWTC-1-confident  ...       1.0  1.00   48.9  48.9  +5.1  -4.0  2017-01-04T10:12:35.600  https://www.gw-openscience.org/eventapi/html/G...
90          90         GW151226      v2  GWTC-1-confident  ...       1.0  1.00   20.5  20.5  +6.4  -1.5  2015-12-26T03:39:29.600  https://www.gw-openscience.org/eventapi/html/G...
91          91         GW151012      v3  GWTC-1-confident  ...       1.0  1.00  35.6  35.6  +10.8  -3.8  2015-10-12T09:55:19.400  https://www.gw-openscience.org/eventapi/html/G...
92          92         GW150914      v3  GWTC-1-confident  ...       1.0  1.00   63.1  63.1  +3.4  -3.0  2015-09-14T09:51:21.400  https://www.gw-openscience.org/eventapi/html/G...

[93 rows x 19 columns]
```

## getting the last gravitational wave detected:
```py
last_gw_name = gw_data.data_gw['Name'][0]
last_gw = gw_data.get_gwave(last_gw_name)

last_gw
```
result:
```sh
{'strain': array([-1.20377769e-18, -1.23316083e-18, -1.19155622e-18, ...,
        -7.84331557e-19, -7.46261544e-19, -7.87365720e-19]),
 'freq': 16000,
 'name': 'GW200322_091133',
 'date': '2020-03-22T09:12:10.300',
 'size': 4030830,
 'detector': 'L1'}
```
## plotting:
```py
import matplotlib.pyplot as plt
plt.style.use('seaborn')

strain = last_gw['strain']

ax, fig = plt.subplots(figsize=(20,12))

plt.title(last_gw_name)
plt.plot(strain)
```
result:
<br/>

<img src='https://raw.githubusercontent.com/reinanbr/gwaves/main/img/plot_1.png'>
<br/>

### plotting the more detector's from same signal:
the detector's used in the data:
```py
gw_data.detectors
```
resut:
```sh
['L1', 'H1', 'V1']
```
getting the strain's
```py
strain_L1 = strain

strain_H1 = gw_data.get_gwave(last_gw_name,detector='H1')['strain']

strain_V1 = gw_data.get_gwave(last_gw_name,detector='V1')['strain']
```
plotting:
```py
ax,fig = plt.subplots(figsize=(32,14))

t = np.linspace(0,32,len(strain_L1))

plt.plot(t,strain_L1,label='L1',c='blue')

plt.plot(t,strain_V1,label='V1',c='green')

plt.plot(t,strain_H1,label='H1',c='red')

plt.legend()
```
result:
<br/>

<img src='https://raw.githubusercontent.com/reinanbr/gwaves/main/img/plot2.png'>

<br/>


### plotting the psd (Power Signal Density):

```py
ax,fig = plt.subplots(figsize=(32,14))

gw_data.plot_psd_from_gwname(last_gw_name,detector="L1")

gw_data.plot_psd_from_gwname(last_gw_name,detector="V1")

gw_data.plot_psd_from_gwname(last_gw_name,detector="H1")
```

result:
<br/>
<img src='https://raw.githubusercontent.com/reinanbr/gwaves/main/img/plot3.png'>






<img src="https://reysofts.com.br/engine/libs/save_table_access_libs.php?lib_name=gwaves">
