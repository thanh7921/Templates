# Introduction

The proliferation of processor using the RISC-V ISA [@spec:riscv-isa-unpriv; @spec:riscv-isa-priv]
has been a boon for the digital chip design industry and academia. Being open-source and
royalty-free, RISC-V offer unique advantages for both researchers and industry players alike
[@report:isa-should-be-free]. Until now, many implementations has surfaced
[@paper:survey-application-class-riscv] serving niches in which more common ISAs like x86 or ARM are
not viable due to high license fee.

Embedded processor is one of the many key application areas of RISC-V, where in specific needs being
implemented with custom extension and instructions to the base RISC-V instruction set. Typical usage
can be found in Internet of Things, smart homes, wearable devices and edge computing
[@paper:riscv-ext-survey]. In embedded system design, there is often the need for embedded
microprocessors to perform tasks such as digital signal processing, encryption/decryption and more
recently acceleration AI model with the push for edge AI.

With such diverse requirements for many different type of systems, designers may find themselves
with the need to be able to customize each design with its own extensions, such as floating point,
vector and cryptographic extensions to meet the application requirements. While previous effort
trying to implement custom extension (such as [@paper:riscv-dsp; @paper:riscv-cnn;
@paper:lu2021surveyriscvsecurityhardware]) has been successful in achieving application-specific
speed-up and energy-efficiency, it has not been easy to adapt those custom extension into other
designs.

As far as I know, there is no generic, cross-compatible interface to integrate a sub-module
implementing specific extension into pre-existing processor core, such that the same sub-module can
be easily reused. The existence of a unified interface to which extension implementations can target
have the potential to accelerate design space exploration, prototyping and research, encouraging
reuse throughout academia and industry.

In this research, I aims to:

- Survey the need of various embedded applications to find common requirements for custom extension
  implementation (i.e. power usage, latency, throughput,...)

- Present a Generic Custom Extension Interface (GCEI) serving as a common interface for the inclusion of
  third-party custom extension implementations.

- Expose drawbacks of the GCEI approach, with advices on recommended use case.

The finished paper will be structured as follows: Problem introduction and goal for this research
are presented in Section I. Related background about RISC-V ISA, its extension and extensibility
will be presented in Section II. The survey result of common requirements for custom extension
implementations will be presented in Section III. The design and specification for the proposed GCEI
will be presented in Section IV along with analysis of its drawbacks. Finally, Section V will delve
into potential use case of GCEI with discussion of the implication its adoption. Section VI will
serve as conclusion and include further research direction.

# Background [@spec:riscv-isa-unpriv]

## RISC-V overview

A RISC-V ISA is defined as a base integer ISA, which must be present in any implementation, plus
optional extensions to the base ISA. The base integer ISAs are very similar to that of the early
RISC processors except with no branch delay slots and with support for optional variable-length
instruction encodings. A base is carefully restricted to a minimal set of instructions sufficient to
provide a reasonable target for compilers, assemblers, linkers, and operating systems (with
additional privileged operations), and so provides a convenient ISA and software toolchain
"skeleton" around which more customized processor ISAs can be built.

Although it is convenient to speak of the RISC-V ISA, RISC-V is actually a family of related ISAs,
of which there are currently four base ISAs. Each base integer instruction set is characterized by
the width of the integer registers and the corresponding size of the address space and by the number
of integer registers. There are two primary base integer variants, RV32I and RV64I, which provide
32-bit or 64-bit address spaces respectively. The RV32E and RV64E subset variants of the RV32I or
RV64I base instruction sets respectively, which have been added to support small microcontrollers,
and which have half the number of integer registers. 

To support more general software development, a set of standard extensions are defined to provide
integer multiply/divide, atomic operations, and single and double-precision floating-point
arithmetic. The base integer ISA is named "I" (prefixed by RV32 or RV64 depending on integer
register width), and contains integer computational instructions, integer loads, integer stores, and
control-flow instructions. The standard integer multiplication and division extension is named "M",
and adds instructions to multiply and divide values held in the integer registers. The standard
atomic instruction extension, denoted by "A", adds instructions that atomically read, modify, and
write memory for inter-processor synchronization. The standard single-precision floating-point
extension, denoted by "F", adds floating-point registers, single-precision computational
instructions, and single-precision loads and stores. The standard double-precision floating-point
extension, denoted by "D", expands the floating-point registers, and adds double-precision
computational instructions, loads, and stores. The standard "C" compressed instruction extension
provides narrower 16-bit forms of common instructions.

## RISC-V extensibility

RISC-V has been designed to support extensive customization and specialization. Each base integer
ISA can be extended with one or more optional instruction-set extensions. An extension may be
categorized as either standard, custom, or non-conforming. For this purpose, each RISC-V
instruction-set encoding space (and related encoding spaces such as the CSRs) is divided into three
disjoint categories: standard, reserved, and custom. Standard extensions and encodings are defined
by RISC-V International; any extensions not defined by RISC-V International are non-standard. Each
base ISA and its standard extensions use only standard encodings, and shall not conflict with each
other in their uses of these encodings. Reserved encodings are currently not defined but are saved
for future standard extensions; once thus used, they become standard encodings. Custom encodings
shall never be used for standard extensions and are made available for vendor-specific non-standard
extensions.

This capability of repacking RISC-V extensions into different standard-compatible global encodings
can be used in a number of ways.

One use-case is developing highly specialized custom accelerators, designed to run kernels from
important application domains. These might want to drop all but the base integer ISA and add in only
the extensions that are required for the task in hand. The base ISAs have been designed to place
minimal requirements on a hardware implementation, and has been encoded to use only a small fraction
of a 32-bit instruction encoding space.

Another use-case is to build a research prototype for a new type of instruction-set extension. The
researchers might not want to expend the effort to implement a variable-length instruction-fetch
unit, and so would like to prototype their extension using a simple 32-bit fixed-width instruction
encoding. However, this new extension might be too large to coexist with standard extensions in the
32-bit space. If the research experiments do not need all the standard extensions, a
standard-compatible global encoding might drop the unused standard extensions and reuse their
prefixes to place the proposed extension in a non-standard location to simplify engineering of the
research prototype. Standard tools will still be able to target the base and any standard extensions
that are present to reduce development time. Once the instruction-set extension has been evaluated
and refined, it could then be made available for packing into a larger variable-length encoding
space to avoid conflicts with all standard extensions.

```{=latex}
\begin{table*}[t]
\centering
\resizebox{0.7\textwidth}{!}{%
\begin{tabular}{|c|l|cccccc|}
\hline
\multirow{2}{*}{Order} &
  \multirow{2}{*}{Steps} &
  \multicolumn{6}{c|}{Timespan (month)} \\ \cline{3-8} 
 &
   &
  \multicolumn{1}{c|}{1} &
  \multicolumn{1}{c|}{2} &
  \multicolumn{1}{c|}{3} &
  \multicolumn{1}{c|}{4} &
  \multicolumn{1}{c|}{5} &
  6 \\ \hline
1 & Interface requirements survey      & \multicolumn{1}{c|}{x} & \multicolumn{1}{c|}{x} & \multicolumn{1}{c|}{x} & \multicolumn{1}{c|}{}  & \multicolumn{1}{c|}{}  &  \\ \hline
2 &
  GCEI design &
  \multicolumn{1}{c|}{} &
  \multicolumn{1}{c|}{x} &
  \multicolumn{1}{c|}{x} &
  \multicolumn{1}{c|}{x} &
  \multicolumn{1}{c|}{} &
   \\ \hline
3 & GCEI implementation and assessment & \multicolumn{1}{c|}{}  & \multicolumn{1}{c|}{}  & \multicolumn{1}{c|}{x} & \multicolumn{1}{c|}{x} & \multicolumn{1}{c|}{x} &  \\ \hline
4 &
  Write paper &
  \multicolumn{1}{c|}{} &
  \multicolumn{1}{c|}{} &
  \multicolumn{1}{c|}{} &
  \multicolumn{1}{c|}{} &
  \multicolumn{1}{c|}{x} &
  x \\ \hline
\end{tabular}%
}
\caption{Proposed timeline for this research}
\label{table:timeline}
\end{table*}
```

# Goals, methods and results

## Interface requirements survey

It is well-known of the trade-off between energy, area and performance in processor design
[@paper:energy-performance-tradeoff; @paper:design-tradeoff; @paper:iot-processor-assessment].
Embedded devices is constrained by energy - to maximize run time, and chip area - to lower cost per
part when deploying large number of device. But at the same time those devices are required to have
some minimum level of performance. Different application will impose different requirements, which
call for different type of custom extensions with unique trade-off point matching its intended use. 

Through learning about existing custom extension implementation, I aim to gain an understanding of
the current landscape of RISC-V embedded devices usage and their most common requirements for
various extension. An understanding by which the process of designing GCEI will be guided.

## GCEI design

Based on requirements collected in the first goals, I will attempt to design versions of the GCEI
with the aim of satisfying the largest set of requirements. Multiple configurations may be proposed
to best align the GCEI specific trade-off that is needed for some applications.

The result of this step will be a collection of different GCEI designs (with possibly multiple
optional configurations). These design are composed of a set of specifications, such as pin
configuration, protocol and communication packet structure. Further RTL implementation is deferred
to the next step.

## GCEI implementation and assessment

At this point, we should have a clear view of the requirements for RISC-V embedded processor,
coupled with some GCEI design variations. The goal of this step is to implement the GCEI
specification into a specialized block with which processor designer can include into their design
early on to enable support for the interface.

Each potential GCEI specification (and their configurations) will be implemented and tested for
energy usage, floor plan area and performance to select the best (or best fit for specific
application) specification and configuration. Energy and floor plan area usage will be assessed with
simulation tool (Synopsys workflow). 

The selected GCEI implementation will be integrated into a popular RISC-V core implementation
(Rocket [@report:rocket-chip-generator]). Performance will be tested with the Coremark benchmark for
different configuration of GCEI. Comparison with existing more natively integrated extension
implementation will be drawn to assess the effect of using GCEI on application-specific performance
criteria.

The result of this step is a reference implementation of GCEI, with configuration advised for
selected applications. Trade off factor such as energy-efficiency, chip area and performance of such
implementation is also investigated and compared with existing design.

# Timeline

I anticipate the timeline for the completion of this research is as presented in table
\ref{table:timeline}. The above mentioned steps is included in the table, with significant overlap
between them. Of the three main step, I foresee the implementation and assessment step taking up the
most amount of time (as it is the most important).
