<script>
import PrimaryButton from '@/components/PrimaryButton'
export default {
    props: [],
    data() {
      return {}
    },
    render(createElement) {
      return createElement(PrimaryButton, {
        nativeOn: {
            click: () => alert('Click!')
        }
      }, 'Hello, world!')
    }
}
</script>
