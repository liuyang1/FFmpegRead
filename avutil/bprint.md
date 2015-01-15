    /**
     * Define a structure with extra padding to a fixed size
     * This helps ensuring binary compatibility with future versions.
     */

    #define FF_PAD_STRUCTURE(name, size, ...) \
    struct ff_pad_helper_##name { __VA_ARGS__ }; \
    typedef struct name { \
        __VA_ARGS__ \
        char reserved_padding[size - sizeof(struct ff_pad_helper_##name)]; \
    } name;

using this MACRO to create padding struct

AVBprint
    FF_PAD_STRUCTURE(AVBPrint, 1024,
        char *str;         /**< string so far */
        unsigned len;      /**< length so far */
        unsigned size;     /**< allocated memory */
        unsigned size_max; /**< maximum allocated memory */
        char reserved_internal_buffer[1];
    )

    av_bprint_init
init bprint struct with size;
size_auto, this will get reserved internal buffer pointer

    av_bprint_alloc
realloc, and copy last data to new buffer
