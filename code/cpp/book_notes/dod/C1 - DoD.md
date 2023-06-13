# Chapter 1 - Data Oriented Design

- Data oriented design coexists with other paradigms.
- World of parallel computing benefits from a data oriented approach of design.
- Bottleneck of competitors is serial thinking and abstractions.

## All about data

- All programs transform data into another form.
- Form of the data may be complex.
- Instructions are data.
- Data is not just data, its data that exists somewhere on the hardware.
- Well formed data guided by the underlying target hardware of the machine.
- Software development methodology founded on the principles of understanding the data and how to transform given knowledge of how a machine will do with this based on quantity / frequency and statistical qualities.

## Data is not the problem domain

- Data does not recognise the concept of an object in any way as data is without meaning.
- Abstractions in code ignore the idea of bytes / CPU pipelines or other hardware features.
- Dod approach does not build the real world problem into code.
- Dod approach prevents the computer dealing with human concepts.
- Object oriented abstractions lead to pieces of unrelated data but are in the same class as some operation requires some context.

- Dod, data is mere *facts* $\to$ interpreted in whatever way necessary to get the output data in the format it needs to be.
- Reducing meanings $\to$ reduce chance of tangling *facts* with *contexts* and thus *likelihood of mixing unrelated data* for the sake of an *operation or two*.

## Data and Statistics

- Data is type, frequency , quantity, shape and probability.
- Not always about cache missing.
- Dod concerned with all aspects of data.
- Dod is centred around data, live data, real data and information data, OOP around problem and solution.
- OOP has no concern for hardware or real world data patterns or quantities.
- Dod assumes no knowledge of the problem.
- Dod avoids waste of resources by never assuming the design needs to exist anywhere other than in a document while it proceeds to provide a solution the current problem.