# climate-data-operators
Minimalistic NodeJS wrapper for [CDO](https://code.mpimet.mpg.de/projects/cdo)

> CDO is a collection of command line Operators to manipulate and
> analyse Climate and NWP model Data.   Supported data formats are GRIB
> 1/2, netCDF 3/4, SERVICE, EXTRA and IEG. There are more than 600
> operators available.

### Usage

#### Install  climate-data-operators
`npm install --save climate-data-operators`

#### Use with distribution-provided CDO (best)
`sudo apt install cdo`

```javascript
const CDO = require('climate-data-operators')
const cdo = new CDO()
cdo.exec(['--help']).then(...)
cdo.exec(['operator', 'argument1', 'argument2'...]).then(...)
```
#### Use with precompiled binary

This scenario is useful where APT is not available (ie: serverless functions).

`climate-data-operators-bin` provides precompiled binary for Ubuntu 18.04 (no external dependency is required)

```bash
# install cdo precompiled binary
npm install --save climate-data-operators-bin

# install eccodes-lib data for file format you need
# see https://www.npmjs.com/package/eccodes-lib
# example:
npm install --save eccodes-data-grib1
npm install --save eccodes-data-grib2
```


```javascript
const CDO = require('climate-data-operators')
const CDOCustomBinary = require('climate-data-operators-bin')
const grib1FormatData = require('eccodes-data-grib1')
const grib2FormatData = require('eccodes-data-grib2')

const demo = async () => {
  const cdo = new CDO(CDOCustomBinary)

  await cdo.loadEcData([ grib1FormatData, grib2FormatData ])

  await cdo.exec(['--help']).then(...)
  await cdo.exec(['operator', 'argument1', 'argument2'...]).then(...)

  // delete temporary format data
  // (needed only when using CDOCustomBinary)
  await cdo.cleanup()
}
demo()
```

### CDO commands
See [CDO Reference Card](https://code.mpimet.mpg.de/projects/cdo/embedded/cdo_refcard.pdf) for a list of usefull commands
