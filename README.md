[![Actions Status](https://github.com/tdulcet/AutoPrimeNet/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/tdulcet/AutoPrimeNet/actions/workflows/ci.yml)

# AutoPrimeNet
The PrimeNet automated assignment handler program for GIMPS

Copyright © 2024 Teal Dulcet

Automatically gets and registers assignments, reports assignment progress and results, uploads proof files to and downloads certification starting values from PrimeNet for the Mlucas, GpuOwl/PRPLL, CUDALucas, mfaktc and mfakto GIMPS programs. Additionally, it can get assignments and report results to mersenne.ca for exponents above the PrimeNet limit of 1G. Supports both Python 2 and 3 and Windows, macOS and Linux. Requires the [Requests library](https://requests.readthedocs.io/en/latest/), which is included with most Python 3 installations. The program will automatically prompt to install Requests on first run if it is not already installed.

Adapted from the PrimeNet Python script from [Mlucas](https://www.mersenneforum.org/mayer/README.html#download2) by [Loïc Le Loarer](https://github.com/llloic11/primenet) and Ernst W. Mayer, which itself was adapted from primetools by [Mark Rose](https://github.com/MarkRose/primetools) and [teknohog](https://github.com/teknohog/primetools).

❤️ Please visit [tealdulcet.com](https://www.tealdulcet.com/) to support this program and my other software development.

## Usage

```
Usage: autoprimenet.py [options]
Use -h/--help to see all options
Use --setup to configure this instance of the program

This program will automatically get and register assignments, report
assignment progress and results, upload proof files to and download
certification starting values from PrimeNet for the Mlucas, GpuOwl/PRPLL,
CUDALucas, mfaktc and mfakto GIMPS programs. It can get assignments and report
results to mersenne.ca for exponents above the PrimeNet limit of 1G. It also
saves its configuration to a 'prime.ini' file by default, so it is only
necessary to give most of the arguments once. The first time it is run, it
will register the current Mlucas/GpuOwl/PRPLL/CUDALucas/mfaktc/mfakto instance
with PrimeNet (see the Registering Options below). Then, it will report
assignment results, get assignments and upload any proof files to PrimeNet on
the --timeout interval, or only once if --timeout is 0. It will additionally
report the progress on the --checkin interval.

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -d, --debug           Output detailed information. Provide multiple times
                        for even more verbose output.
  -w WORKDIR, --workdir=WORKDIR
                        Working directory with the local file from this
                        program, Default: . (current directory)
  -D DIRS, --dir=DIRS   Directories relative to --workdir with the work and
                        results files from the GIMPS program. Provide once for
                        each worker. It automatically sets the --cpu-num
                        option for each directory.
  -i WORKTODO_FILE, --work-file=WORKTODO_FILE
                        Work file filename, Default: 'worktodo.txt'
  -r RESULTS_FILE, --results-file=RESULTS_FILE
                        Results file filename, Default: 'results.json.txt' for
                        mfaktc/mfakto or 'results.txt' otherwise
  -L LOGFILE, --logfile=LOGFILE
                        Log file filename, Default: 'autoprimenet.log'
  -l LOCALFILE, --config-file=LOCALFILE
                        Local configuration file filename, Default:
                        'prime.ini'
  --archive-proofs=ARCHIVE_DIR
                        Directory to archive PRP proof files after upload,
                        Default: none
  -u USER_ID, --username=USER_ID
                        GIMPS/PrimeNet User ID. Create a GIMPS/PrimeNet
                        account: https://www.mersenne.org/update/. If you do
                        not want a PrimeNet account, you can use ANONYMOUS.
  -T WORK_PREFERENCE, --worktype=WORK_PREFERENCE
                        Type of work, Default: 150. Supported work
                        preferences: 2 (Trial factoring), 4 (P-1 factoring),
                        12 (Trial factoring GPU), 100 (First time LL tests),
                        101 (Double-check LL tests), 102 (World record LL
                        tests), 104 (100M digit LL tests), 106 (Double-check
                        LL tests with zero shift count), 150 (First time PRP
                        tests), 151 (Double-check PRP tests), 152 (World
                        record PRP tests), 153 (100M digit PRP tests), 154
                        (Smallest available first time PRP that needs P-1
                        factoring), 155 (Double-check using PRP with proof),
                        156 (Double-check using PRP with proof and nonzero
                        shift count), 160 (First time PRP on Mersenne
                        cofactors), 161 (Double-check PRP on Mersenne
                        cofactors). Provide once to use the same worktype for
                        all workers or once for each worker to use different
                        worktypes. Not all worktypes are supported by all the
                        GIMPS programs.
  --cert-work           Get PRP proof certification work, Default: none. Not
                        yet supported by any of the GIMPS programs.
  --cert-work-limit=CERT_CPU_LIMIT
                        PRP proof certification work limit in percentage of
                        CPU or GPU time, Default: 10%. Requires the --cert-
                        work option.
  --min-exp=MIN_EXP     Minimum exponent to get from PrimeNet or TF1G (2 -
                        9,999,999,999). TF1G assignments are supported by
                        setting this flag to 1,000,000,000 or above.
  --max-exp=MAX_EXP     Maximum exponent to get from PrimeNet or TF1G (2 -
                        9,999,999,999)
  --bit-min=BIT_MIN     Minimum bit level of TF1G assignments to fetch
  --bit-max=BIT_MAX     Maximum bit level of TF1G assignments to fetch
  -m, --mlucas          Get assignments for Mlucas.
  -g, --gpuowl          Get assignments for GpuOwl. PRPLL is not yet fully
                        supported.
  --cudalucas           Get assignments for CUDALucas.
  --mfaktc              Get assignments for mfaktc.
  --mfakto              Get assignments for mfakto.
  --num-workers=NUM_WORKERS
                        Number of workers (CPU Cores/GPUs), Default: 1
  -c CPU, --cpu-num=CPU
                        CPU core or GPU number to get assignments for,
                        Default: 0. Deprecated in favor of the --dir option.
  -n NUM_CACHE, --num-cache=NUM_CACHE
                        Number of assignments to cache, Default: 0. Deprecated
                        in favor of the --days-work option.
  -W DAYS_OF_WORK, --days-work=DAYS_OF_WORK
                        Days of work to queue ((0-180] days), Default: 1 day
                        for mfaktc/mfakto or 3 days otherwise. Increases
                        num_cache when the time left for all assignments is
                        less than this number of days.
  --force-pminus1=TESTS_SAVED
                        Force P-1 factoring before LL/PRP tests and/or change
                        the default PrimeNet PRP and P-1 tests_saved value.
  --pminus1-threshold=PM1_MULTIPLIER
                        Retry the P-1 factoring before LL/PRP tests only if
                        the existing P-1 bounds are less than the target
                        bounds (as listed on mersenne.ca) times this
                        threshold/multiplier. Requires the --force-pminus1
                        option.
  --force-pminus1-bounds=PM1_BOUNDS
                        Force using the 'MIN', 'MID' or 'MAX' optimal P-1
                        bounds (as listed on mersenne.ca) for P-1 tests. For
                        Mlucas, this will rewrite Pfactor= assignments to
                        Pminus1=. For GpuOwl, this will use a nonstandard
                        Pfactor= format to add the bounds. Can be used in
                        combination with the --force-pminus1 option.
  --convert-ll-to-prp   Convert all LL assignments to PRP. This is for use
                        when registering assignments.
  --convert-prp-to-ll   Convert all PRP assignments to LL. This is
                        automatically enabled for first time PRP assignments
                        when the --worktype option is for a first time LL
                        worktype.
  --no-report-100m      Do not report any prime results for exponents greater
                        than or equal to 100 million digits. You must setup
                        another method to notify yourself, such as setting the
                        notification options below.
  --checkin=HOURS_BETWEEN_CHECKINS
                        Hours to wait between sending assignment progress and
                        expected completion dates (1-168 hours), Default: 1
                        hours.
  -t TIMEOUT, --timeout=TIMEOUT
                        Seconds to wait between updates, Default: 3600 seconds
                        (1 hour). Users with slower internet may want to set a
                        larger value to give time for any PRP proof files to
                        upload. Use 0 to update once and exit.
  -s, --status          Output a status report and any expected completion
                        dates for all assignments and exit.
  --upload-proofs       Report assignment results, upload all PRP proofs and
                        exit. Requires PrimeNet User ID.
  --recover             Report assignment results, recover all assignments and
                        exit. This will overwrite any existing work files.
  --recover-all         The same as --recover, except for PrimeNet it will
                        additionally recover expired assignments and for
                        mersenne.ca it will recover all assignments for all
                        systems/workers to the first worker. This will
                        overwrite any existing work files.
  --register-exponents  Prompt for all parameters needed to register one or
                        more specific exponents and exit.
  --unreserve=EXPONENT  Unreserve the exponent and exit. Use this only if you
                        are sure you will not be finishing this exponent.
  --unreserve-all       Report assignment results, unreserve all assignments
                        and exit.
  --no-more-work        Prevent this program from getting new assignments and
                        exit.
  --resume-work         Resume getting new assignments after having previously
                        run the --no-more-work option and exit.
  --ping                Ping the PrimeNet server, show version information and
                        exit.
  --no-color            Do not use color in output.
  --setup               Prompt for all the options that are needed to setup
                        this program and exit.

  Registering Options:
    Sent to PrimeNet/GIMPS when registering. It will automatically send
    the progress, which allows the program to then be monitored on the
    GIMPS website CPUs page (https://www.mersenne.org/cpus/), just like
    with Prime95/MPrime. This also allows the program to get much smaller
    Category 0 and 1 exponents, if it meets the other requirements
    (https://www.mersenne.org/thresholds/).

    -H COMPUTER_ID, --hostname=COMPUTER_ID
                        Optional computer name, Default: example
    --cpu-model=CPU_BRAND
                        Processor (CPU) model, Default: cpu.unknown
    --features=CPU_FEATURES
                        CPU features, Default: ''
    --frequency=CPU_SPEED
                        CPU frequency/speed (MHz), Default: 1000 MHz
    --memory=MEMORY     Total physical memory (RAM) (MiB), Default: 1024 MiB
    --max-memory=DAY_NIGHT_MEMORY
                        Configured day/night P-1 stage 2 memory (MiB),
                        Default: 921 MiB (90% of physical memory). Required
                        for P-1 assignments.
    --max-disk-space=WORKER_DISK_SPACE
                        Configured disk space limit per worker to store the
                        proof interim residues files for PRP tests
                        (GiB/worker), Default: 0.0 GiB/worker. Use 0 to not
                        send.
    --l1=CPU_L1_CACHE_SIZE
                        L1 Data Cache size (KiB), Default: 8 KiB
    --l2=CPU_L2_CACHE_SIZE
                        L2 Cache size (KiB), Default: 512 KiB
    --l3=CPU_L3_CACHE_SIZE
                        L3 Cache size (KiB), Default: 0 KiB
    --cores=NUM_CORES   Number of physical CPU cores, Default: 1
    --hyperthreads=CPU_HYPERTHREADS
                        Number of CPU threads per core (0 is unknown),
                        Default: 0. Choose 1 for non-hyperthreaded and 2 or
                        more for hyperthreaded.
    --hours=CPU_HOURS   Hours per day you expect the GIMPS program will run (1
                        - 24), Default: 24 hours. Used to give better
                        estimated completion dates.

  Notification Options:
    Optionally configure this program to automatically send an e-mail/text
    message notification if there is an error, if the GIMPS program has
    stalled, if the available disk space is low or if it found a new
    Mersenne prime. Send text messages by using your mobile providers
    e-mail to SMS or MMS gateway. Use the --test-email option to verify
    the configuration.

    --to=TOEMAILS       To e-mail address. Use multiple times for multiple
                        To/recipient e-mail addresses. Defaults to the --from
                        value if not provided.
    -f FROMEMAIL, --from=FROMEMAIL
                        From e-mail address
    -S SMTP, --smtp=SMTP
                        SMTP server. Optionally include a port with the
                        'hostname:port' syntax. Defaults to port 465 with
                        --tls and port 25 otherwise.
    --tls               Use a secure connection with SSL/TLS
    --starttls          Upgrade to a secure connection with StartTLS
    -U EMAIL_USERNAME, --email-username=EMAIL_USERNAME
                        SMTP server username
    -P EMAIL_PASSWORD, --email-password=EMAIL_PASSWORD
                        SMTP server password
    --test-email        Send a test e-mail message and exit
```


## Contributing

Pull requests welcome! Ideas for contributions:

* Support more GIMPS programs.
* Support setting more of the program options.
* Improve the error handling of PrimeNet API calls.
* Hide any prompts that are not applicable when using the `--setup` option. (requested by James)
* Check for new results to submit and proof files to upload when the results file is updated.
* Improve the performance.
* Support reporting interim residues.
* Support downloading certification assignments.
	* Waiting on support from one of more of the GIMPS programs.
* Add automatic update check and notification.
* Localize the program and translate the output into other languages (see [here](https://mersenneforum.org/showthread.php?t=27046)).
* Adapt Loïc Le Loarer's [test suite](https://github.com/llloic11/primenet/tree/main/tests).
* Add an optional GUI using [Tk](https://en.wikipedia.org/wiki/Tk_(software)) and the [tkinter library](https://docs.python.org/3/library/tkinter.html)
* Add docstrings to all functions
* Add an option to show debugging information
* Support submitting P-1 results for Fermat numbers

Thanks to [Daniel Connelly](https://github.com/Danc2050) for updating the PrimeNet Python script from Mlucas to eliminate the password requirement by getting assignments using the [PrimeNet API](http://v5.mersenne.org/v5design/v5webAPI_0.97.html) and to support reporting the assignment results and progress for CUDALucas using the PrimeNet API!

Thanks to Isaac Terrell for providing the needed PRP proof files to test the proof file uploading feature.

Thanks to [Tyler Busby](https://github.com/brubsby) for updating AutoPrimeNet to support mfaktc/mfakto and getting assignments and reporting results to mersenne.ca for exponents above the PrimeNet limit of 1G.

