// Design of a filesystem in C++
// Author: Yilong Zhang
// Date: 4/20/2017

// use enum class rather than enum to make it more type-safe and thread safe
enum class FileType
{
    Regular,
    Dir
};

class SuperBlock
{
    private: 
        // some metadata here
        string magicNumber;
        time_t mountTime;
        string volumeName;
        
        unsigned int totalInodeCnt;
        unsigned int freeInodeCnt;
        unsigned int inodeSize;
        
        unsigned int totalBlockCnt;
        unsigned int availableBlockCnt;
        unsigned int blockSize;
        Inode* root;
    public:
        SuperBlock(Inode* r) : root(r){}
        // setters and getters;
};

// virtual class, never instantiate it. To be featured by children classes.
class Inode
{
    protected:
        int inodeId;
        bool inUse;
        string owner;
        int accessMode;
        int fileSize;
        time_t accessTime;
        FileType ft;
    public:
        Inode(int id, bool use, string ow, int mode, int fsize, FileType ft){}
        virtual FileType inodeType() = 0;
        virtual ~Inode(){}
        // other setters and getters
};

class InodeFile : public Inode
{
    private:
        int numberHardLinks;
        uint16_t directPointer[6];
        uint16_t indirectPointer[2];
        uint16_t doubleIndirectPointer;   
    public:
        InodeFile(){}
        FileType inodeType() override;
};

class InodeDir : public Inode
{
    private:
        int availability;
	uint16_t pointerToBlock;
    public:
        InodeDir(){}
        FileType inodeType() override;
};

class Dentry
{
    char names[7][64];
    int inodeId[7];
};

class FileDiscriptor
{
    int fdId;
    int inodeId;
    int blockId;
    int offset;
};

class FileSystem
{
    SuperBlock* sb;
    Inode* inodes;
    void* dataBlocks;
    
    public:
        FileSystem(){}
        FileSystem* fs_format(string& path);
        FileSystem* fs_mount(string& path);
        bool fs_unmount(FileSystem* fs);
        bool fs_create(FileSystem* fs, const string& path, FileType ft);
        int fs_open(FileSystem* fs, string path);
        bool fs_close(FileSystem* fs, int fd);
        int fs_read(FileSystem* fs, int fd, void* dst, size_t nbytes);
        int fs_write(FileSystem* fs, int fd, void* src, size_t nbytes);
};
