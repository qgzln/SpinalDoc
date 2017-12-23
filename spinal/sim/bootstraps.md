---
type: homepage
title: "SpinalSim boot"
tags: [user guide]
keywords: spinal, user guide
last_updated: Apr 19, 2016
sidebar: spinal_sidebar
toc: true
permalink: /spinal/sim/bootstraps/
---

## Introduction

There is an example hardware defintion + testbench :

```scala
//Your hardware toplevel
import spinal.core._
class TopLevel extends Component{
  ...
}

//Your toplevel tester
import spinal.sim._
import spinal.core.sim._

object DutTests {
  def main(args: Array[String]): Unit = {
    SimConfig(new TopLevel).withWave.doManagedSim{ dut =>
      //Simulation code here
    }
  }
}
```

## Configuration

SimConfig(xxx) will returnn your a SimConfig class on which you can call multiple function to configure your simulation :

| Syntax                            | Description                                                                         |
| --------------------------------- | ----------------------------------------------------------------------------------- |
| withWave                          |  Enable the simulation wave capture                                         |
| withConfig(SpinalConfig)          |  Specify the SpinalConfig that should be use to generate the hardware                       |
| allOptimisation                   |  Enable all the RTL compilation optimisation to reduce simulation time (will increase compilation time)              |
| workspacePath(path)               | Change the folder where the sim files are generated |

For example :

```scala
val spinalConfig = SpinalConfig(defaultClockDomainFrequency = FixedFrequency(10 MHz))

SimConfig(new TopLevel)
  .withConfig(spinalConfig)
  .withWave
  .allOptimisation
  .workspacePath("~/tmp")
  .doManagedSim{ dut =>
  //Simulation code here
}
```

Note that by default, the simulation will work into the simWorkspace/xxx folders. You can override the simWorkspace location by setting the SPINALSIM_WORKSPACE environnement variable.

## Running multiple tests on the same hardware

```scala
 val compiled = SimConfig(new Dut).withWave.compile()

 compiled.doManagedSim("testA"){ dut =>
    //Simulation code here
 }

 compiled.doManagedSim("testB"){ dut =>
    //Simulation code here
 }
```