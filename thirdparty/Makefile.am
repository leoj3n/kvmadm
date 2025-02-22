# Author: Tobi Oetiker <tobi@oetiker.ch>
# License: Public Domain

AUTOMAKE_OPTIONS =  foreign

THIRDPARTY_DIR := $(shell pwd)

THIRDPARTY_DIST := $(shell test -d CPAN && find CPAN -type d -name ".??*" -prune -o -not -name ".*" -a -not -name "*~" -a -not -name "*.tmp"  -a -type f -print )

EXTRA_DIST = $(THIRDPARTY_DIST) $(wildcard bin/cpanm)

all-local:
	$(AM_V_GEN)[ "$(MISSING_PERL_MODULES)" == "" ] || $(GMAKE) touch

touch: CPAN/touch ../config.status
	$(AM_V_GEN)echo $(MISSING_PERL_MODULES) | PERL_CPANM_HOME=$(THIRDPARTY_DIR) xargs $(PERL) $(THIRDPARTY_DIR)/bin/cpanm -q --notest --local-lib-contained $(THIRDPARTY_DIR)  --mirror file://$(THIRDPARTY_DIR)/CPAN --mirror-only
	$(AM_V_GEN)touch touch

CPAN/touch: ../PERL_MODULES
	echo "POPULATING OUR LOCAL micro CPAN"
	$(AM_V_GEN)$(URL_CAT) http://cpanmin.us | PERL_CPANM_HOME=$(THIRDPARTY_DIR) $(PERL) - -q --notest --local-lib $(THIRDPARTY_DIR) --save-dists $(THIRDPARTY_DIR)/CPAN --force App::cpanminus
	$(AM_V_GEN)PERL_CPANM_HOME=$(THIRDPARTY_DIR)/Ore $(THIRDPARTY_DIR)/bin/cpanm -q --notest  --local-lib $(THIRDPARTY_DIR)/Ore File::Which OrePAN
	$(AM_V_GEN)cat ../PERL_MODULES | PERL_CPANM_HOME=$(THIRDPARTY_DIR) xargs $(PERL) $(THIRDPARTY_DIR)/bin/cpanm -q --self-contained --notest --local-lib-contained $(THIRDPARTY_DIR) --save-dists $(THIRDPARTY_DIR)/CPAN
	$(AM_V_GEN)PERL5LIB=$(THIRDPARTY_DIR)/Ore/lib/perl5 $(THIRDPARTY_DIR)/Ore/bin/orepan_index.pl --repository $(THIRDPARTY_DIR)/CPAN
# Ore fails to extract the version form DBI
	$(AM_V_GEN)gunzip -c CPAN/modules/02packages.details.txt.gz | perl -pe 's{^(DBI\s+)undef(\s+\S+/DBI-)(\d+\.\d+)(\.tar)}{$$1$$3$$2$$3$$4}' |  gzip | cat > x.gz && mv x.gz CPAN/modules/02packages.details.txt.gz
	$(AM_V_GEN)touch CPAN/touch

clean-local:
	ls -1 | $(GREP) -v Makefile. | $(GREP) -v CPAN |$(GREP) -v bin| xargs rm -rf

distclean-local:
	ls -1 | $(GREP) -v Makefile. | xargs rm -rf
