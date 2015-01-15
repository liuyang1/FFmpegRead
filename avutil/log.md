log.h

AVClass 

    /**
     * Describe the class of an AVClass context structure. That is an
     * arbitrary struct of which the first field is a pointer to an
     * AVClass struct (e.g. AVCodecContext, AVFormatContext etc.).
     */
    typedef struct AVClass {
        /**
         * The name of the class; usually it is the same name as the
         * context structure type to which the AVClass is associated.
         */
        // type of class
        const char* class_name;

        /**
         * A pointer to a function which returns the name of a context
         * instance ctx associated with the class.
         */
        const char* (*item_name)(void* ctx);

        /**
         * a pointer to the first option specified in the class if any or NULL
         *
         * @see av_set_default_options()
         */
        const struct AVOption *option;

        /**
         * LIBAVUTIL_VERSION with which this structure was created.
         * This is used to allow fields to be added without requiring major
         * version bumps everywhere.
         */

        int version;

        /**
         * Offset in the structure where log_level_offset is stored.
         * 0 means there is no such variable
         */
        int log_level_offset_offset;

        /**
         * Offset in the structure where a pointer to the parent context for
         * logging is stored. For example a decoder could pass its AVCodecContext
         * to eval as such a parent context, which an av_log() implementation
         * could then leverage to display the parent context.
         * The offset can be NULL.
         */
        int parent_log_context_offset;

        /**
         * Return next AVOptions-enabled child or NULL
         */
        void* (*child_next)(void *obj, void *prev);

        /**
         * Return an AVClass corresponding to the next potential
         * AVOptions-enabled child.
         *
         * The difference between child_next and this is that
         * child_next iterates over _already existing_ objects, while
         * child_class_next iterates over _all possible_ children.
         */
        const struct AVClass* (*child_class_next)(const struct AVClass *prev);

        /**
         * Category used for visualization (like color)
         * This is only set if the category is equal for all objects using this class.
         * available since version (51 << 16 | 56 << 8 | 100)
         */
        AVClassCategory category;

        /**
         * Callback to return the category.
         * available since version (51 << 16 | 59 << 8 | 100)
         */
        AVClassCategory (*get_category)(void* ctx);

        /**
         * Callback to return the supported/allowed ranges.
         * available since version (52.12)
         */
        int (*query_ranges)(struct AVOptionRanges **, void *obj, const char *key, int flags);
    } AVClass;

log level
    /**
     * Print no output.
     */
    #define AV_LOG_QUIET    -8

    /**
     * Something went really wrong and we will crash now.
     */
    #define AV_LOG_PANIC     0

    /**
     * Something went wrong and recovery is not possible.
     * For example, no header was found for a format which depends
     * on headers or an illegal combination of parameters is used.
     */
    #define AV_LOG_FATAL     8

    /**
     * Something went wrong and cannot losslessly be recovered.
     * However, not all future data is affected.
     */
    #define AV_LOG_ERROR    16

    /**
     * Something somehow does not look correct. This may or may not
     * lead to problems. An example would be the use of '-vstrict -2'.
     */
    #define AV_LOG_WARNING  24

    /**
     * Standard information.
     */
    #define AV_LOG_INFO     32

    /**
     * Detailed information.
     */
    #define AV_LOG_VERBOSE  40

    /**
     * Stuff which is only useful for libav* developers.
     */
    #define AV_LOG_DEBUG    48

    #define AV_LOG_MAX_OFFSET (AV_LOG_DEBUG - AV_LOG_QUIET)

cross platform colorful logging support
- log.c scope flag *use_color*
- check_color_terminal set
- colored_fputs dynamic output prefix of logging line

santize
format unprintable char to '?' char
