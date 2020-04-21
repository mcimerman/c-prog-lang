# Internal vs external linkage

Static objects with the keyword `static` are of internal linkage, meaning they
are not seen from other compilation units.  Static objects without the keyword
`static` are implicitly external.

Use `extern` keyword for objects that are defined in a different compilation
unit.

Example:

	$ cc linkage.c ext.c
	Undefined symbols for architecture x86_64:
	  "_si", referenced from:
	      _main in linkage-917564.o
	ld: symbol(s) not found for architecture x86_64
	clang: error: linker command failed with exit code 1 (use -v to see
	invocation)

Also note that each object must have exactly one definition.  For objects with
internal linkage, this rule applies separately to each translation unit, because
internally-linked objects are unique to a translation unit.