<template>
    <div class="app-container">
        <el-form label-width="120px">
            <el-form-item label="讲师名称">
                <el-input v-model="teacher.name" />
            </el-form-item>
            <el-form-item label="讲师排序">
                <el-input-number v-model="teacher.sort" controls-position="right" :min="0" />
            </el-form-item>
            <el-form-item label="讲师头衔">
                <el-select v-model="teacher.level" clearable placeholder="请选择">
                    <el-option :value="1" label="高级讲师" />
                    <el-option :value="2" label="首席讲师" />
                </el-select>
            </el-form-item>
            <el-form-item label="讲师资历">
                <el-input v-model="teacher.career" />
            </el-form-item>
            <el-form-item label="讲师简介">
                <el-input v-model="teacher.intro" :rows="10" type="textarea" />
            </el-form-item>

            <!-- 讲师头像：TODO -->

            <el-form-item>
                <el-button :disabled="saveBtnDisabled" type="primary" @click="saveOrUpdate">保存</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>


<script>
import teacherApi from "@/api/edu/teacher";
export default {
    data() {
        return {
            teacher: {
                name: "",
                sort: 0,
                level: 1,
                career: "",
                intro: "",
                avatar: "",
            },
            saveBtnDisabled: false,
        };
    },
    watch: {
        $route(to, from) {
            this.init();
        },
    },
    created() {
        this.init();
    },

    methods: {
        init() {
            //判断路径有id值,做修改
            if (this.$route.params && this.$route.params.id) {
                const id = this.$route.params.id;
                this.getInfo(id);
            } else {
                this.teacher = {};
            }
        },
        getInfo(id) {
            teacherApi.getTeacherInfoById(id).then((response) => {
                this.teacher = response.data.teacher;
            });
        },
        saveOrUpdate() {
            if (!this.teacher.id) {
                //添加
                this.saveTeacher();
            } else {
                //修改
                this.updateTeacherInfo();
            }
        },
        saveTeacher() {
            teacherApi
                .addTeacher(this.teacher)
                .then((response) => {
                    this.$message({
                        type: "success",
                        message: "添加成功!",
                    });
                    this.$router.push({ path: "/teacher/list" });
                })
                .catch((err) => { });
        },

        updateTeacherInfo() {
            teacherApi
                .updateTeacher(this.teacher)
                .then((response) => {
                    this.$message({
                        type: "success",
                        message: "修改成功!",
                    });
                    this.$router.push({ path: "/teacher/list" });
                })
                .catch((err) => { });
        },
    },
};
</script>
